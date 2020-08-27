---
layout: post
title: Exploring scikit-learn
date: 2020-8-28 0:34:00-0200
description: 
comments: false
---

<p style="text-align: center;"><font size="+3">Introduction</font></p>
For my current NEAT project, I read the paper [*A Comparative Study of Supervised Machine Learning Algorithms for Stock Market Trend Prediction*](https://ieeexplore.ieee.org/abstract/document/8473214?casa_token=mA1Va18Dm6kAAAAA:v_6_aQSag5JUXPvV3uPm-BYIVUfWLtCD5HZFDXopj5UUDriA460pLKGfCr99nKgQEYCw8a-GAQ) to learn  how others approached stock market prediction using supervised machine learning algorithms. This paper compared the accuracy between Support Vector Machine, Random Forest, K-Nearest Neighbor, Naive Bayes, and SoftMax algorithms. After reading about these different algorithms, I took it upon myself to learn more about the scikit-learn python library. I am learning how to use scikit-learn because it will give me the ability to implement some the algorithms outlined in the paper. 

<p style="text-align: center;"><font size="+3">Random Forest</font></p>
The first algorithm I sought to better understand was the Random Forest algorithm. This algorithm is consisting of multiple decision trees. A decision tree learns simple decision rules from the data and produces a output. Each tree is used on a different sub sample of the dataset and averaging is used to improve accuracy. 

The decision trees all “vote” by producing outputs, and whatever the most popular output is becomes the algorithms output. I first imported my data as a Pandas data frame and split it up into a training section and a testing section. I then created a Random Forest algorithm using the scikit-learn random forest classifier. After running the algorithm for a few times, I noticed that the accuracy was unusually high. After some investigation, I discovered that the algorithms were predicting the stock price movement for the current day instead of one day in the future. After fixing the problem, the accuracy on the testing dataset stayed around 50%. Here is the code that I wrote:




{% highlight python linenos %}


import csv
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn import metrics
from sklearn.metrics import classification_report

#this opens the file and puts the data in a pandas dataframe
data = pd.read_csv('RandomForest.csv')
data.head()

#setting the size of the training dataset
training_size = .7
split = int(len(data)*.7)

#Identifying all of the inputs and the expected output
X=data[['Open','High','Low','Close','Volume','Accumulation Distribution Line','MACD','Chaikan Oscillator (CHO)','Highest closing price (5 days)',
'Lowest closing price (days)','Stochastic %K (5 days)','%D','Volume Price Trend (VPT)','Williams %R (14 days)','Relative Strength Index','Momentum (10 days)',
'Price rate of change (PROC)','Volume rate of change (VROC)','On Balance Volume (OBV)']]
y=data['Outputs']

#splitting the dataframe into a training and testing dataset
X_train = X[:split]
X_test = X[split:]
y_train = y[:split]
y_test = y[split:]

#create a the random forest with 500 decision trees
clf=RandomForestClassifier(n_estimators=500) 

#train the model on the training data
clf.fit(X_train,y_train)

#making predictions on the testing data
y_pred=clf.predict(X_test)

#printing the testing accuracy
print("Accuracy:",metrics.accuracy_score(y_test, y_pred))

{% endhighlight %}






<p style="text-align: center;"><font size="+3">K-Nearest Neighbors</font></p>
The next algorithm I studied was the K-Nearest Neighbors algorithm. The nearest neighbor method looks for the most similar trading days. The predefined number of the most similar trading days become the nearest neighbors. Just like the decision trees in the Random Forest algorithm, the nearest neighbors "vote". Instead of using decision trees to produce an output, the K-Nearest Neighbor algorithm uses the actual expected output for the most similar trading days. The most popular output becomes of the neighbors becomes the algorithms output. If 5 of the nearest neighbors had the closing stock price go up and 3 neighbors had the stock price go down, the prediction will be that the stock price goes up. 

While studying this algorithm, I learned that K-Nearest Neighbor algorithms performs better with a lower number of features/inputs. To reduce the size of my dataset, I first normalized it with the standard scaler tool from scikit-learn. I then used PCA to shrink the number of inputs in my dataset down to two while preserving the variance in the dataset.  Here is my K-Nearest Neighbors code:











{% highlight python linenos %}

import csv
import pandas as pd
from sklearn import metrics
from sklearn.metrics import classification_report
from sklearn.neighbors import KNeighborsClassifier
from sklearn.decomposition import PCA
from sklearn.preprocessing import StandardScaler

#this opens the CSV File
data = pd.read_csv('RandomForest.csv')
data.head()

#preprocessing and normalizing
Features = ['Open','High','Low','Close','Volume','Accumulation Distribution Line','MACD','Chaikan Oscillator (CHO)','Highest closing price (5 days)',
'Lowest closing price (days)','Stochastic %K (5 days)','%D','Volume Price Trend (VPT)','Williams %R (14 days)','Relative Strength Index','Momentum (10 days)',
'Price rate of change (PROC)','Volume rate of change (VROC)','On Balance Volume (OBV)']

X = data.loc[:, Features].values
Y = data.loc[:,['Outputs']].values

#normalizing the dataset
X = StandardScaler().fit_transform(X)

#performing PCA
PCA = PCA(n_components=2)
Components = PCA.fit_transform(X)
ComponentDf = pd.DataFrame(data=Components)

#setting the size of the training set
training_size = .8
split = int(len(data)*training_size)

#splitting up the dataset
X_train = pd.DataFrame(data=ComponentDf[:split])
X_test = pd.DataFrame(data=ComponentDf[split:])
Y_train = pd.DataFrame(data=Y[:split])
Y_test = pd.DataFrame(data=Y[split:])

#create the K Nearest Neighbor Algorithm and running the algorithm
clf=KNeighborsClassifier(n_neighbors=5)
clf.fit(X_train, Y_train.values.ravel())
Y_pred=clf.predict(X_test)

#printing the testing accuracy 
print("Accuracy:",metrics.accuracy_score(Y_test, Y_pred))


{% endhighlight %}

