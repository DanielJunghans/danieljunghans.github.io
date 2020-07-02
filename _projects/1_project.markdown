---
layout: page
title: Neat Project
description: Predicting Stock Closing Prices with NEAT
img: /assets/img/graph5.png
---


<p style="text-align: center;"><font size="+3">Introduction</font></p>
I began this research project with a simple question: can I evolve an artificial neural network (ANN) to accurately predict the closing price of a stock? For this project, I am using the Neuro Evolution of Augmenting Topologies (NEAT) algorithm to evolve my ANNs. The inputs for my ANNs consist of historical trading data for Tesla and 19 technical indicators. To learn more about artificial neural networks, NEAT, or the financial data I'm using, check out my blog post [here](http://danieljunghans.com/blog/2020/NEAT/).

<p style="text-align: center;"><font size="+3">Methods</font></p>
<h4>Training and Testing Data</h4>
The first thing my program does is split my financial dataset into two pieces. The first 70% of the dataset becomes the training data, and the last 30% of the dataset becomes the testing data. <br />

<h4>Measuring Performance</h4>
The training dataset is first randomly sampled. Then the ANNs produce outputs (closing price predictions) from the sampled data. While the ANNs produce outputs, their error
is determined by subtracting their output from the expected output (closing stock price). 
<br />
<h4>Speciation and Reproduction</h4>
Once the ANNs have completed the training dataset, they are divided up into species based on genomic distance (this is a measure of how similar genomes are based on their structure i.e. nodes and connections). After all the ANNs have been put in a species, parents are selected to produce offspring through sexual and asexual reproduction. These offspring may be mutated and will be the next generation of ANNs. This cycle of measuring performance, speciation, selection, reproduction, and mutation will continue for *N* generations. 
<br />
<h4>Testing Data</h4>
After *N* generations, my code checks to see if the best ANN meets my performance threshold.  

•	If the best performing ANN does not meet this threshold, the ANNs continue to run on the training data for *N* generations. 

•	When the best ANN meets this performance threshold, it is run on the entire testing dataset. The outputs (stock price predictions) are recorded, and my code checks to see if the best neural network is good enough to stop running the program.  

<p style="text-align: center;"><font size="+3">Results</font></p>

My first experiment was to see if my ANNs could predict the closing stock price only using one activation function and normalized data. I graphed the best ANNs testing data predictions for all 50 runs. As a baseline, I chose to use yesterday's closing price as the current day's prediction. The baseline will be represented with red dots in the following figures. 

<div class="img">
    <img class="col three" src="{{ site.baseurl }}/assets/img/graph1.PNG">
</div>

<div class="img">
    <img class="col three" src="{{ site.baseurl }}/assets/img/graph2.PNG">
</div>

<div class="img">
    <img class="col three" src="{{ site.baseurl }}/assets/img/graph3.PNG">
</div>

[Here is a link to the github repository!](https://github.com/DanielJunghans/NEAT_Project)




