# Mechanisms of Action Competition
*Can you improve the algorithm that classifies drugs based on their biological activity?*

## 1.00 Introduction
### 1.01 What is the Mechanism of Action (MoA) of a drug? And why is it important?

In the past, scientists derived drugs from natural products or were inspired by traditional remedies. Very common drugs, such as paracetamol, known in the US as acetaminophen, were put into clinical use decades before the biological mechanisms driving their pharmacological activities were understood. Today, with the advent of more powerful technologies, drug discovery has changed from the serendipitous approaches of the past to a more targeted model based on an understanding of the underlying biological mechanism of a disease. In this new framework, scientists seek to identify a protein target associated with a disease and develop a molecule that can modulate that protein target. As a shorthand to describe the biological activity of a given molecule, scientists assign a label referred to as mechanism-of-action or MoA for short.

### 1.02 How do we determine the MoAs of a new drug?

One approach is to treat a sample of human cells with the drug and then analyze the cellular responses with algorithms that search for similarity to known patterns in large genomic databases, such as libraries of gene expression or cell viability patterns of drugs with known MoAs.

In this competition, we have access to a unique dataset that combines gene expression and cell viability data. The data are based on a new technology that measures simultaneously (within the same samples) human cells’ responses to drugs in a pool of 100 different cell types (thus solving the problem of identifying ex-ante, which cell types are better suited for a given drug). In addition, you will have access to MoA annotations for more than 5,000 drugs in this dataset.

As is customary, the dataset has been split into testing and training subsets. Hence, our task is to use the training dataset to develop an algorithm that automatically labels each case in the test set as one or more MoA classes. Note that since drugs can have multiple MoA annotations, the task is formally a multi-label classification problem.

### 1.03 Evaluation
For every `sig_id` we predict the probability that the sample had a positive response for each <MoA> target. For <a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{300}&space;\large&space;N" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\dpi{300}&space;\large&space;N" title="\large N" /></a> `sig_id` rows and <a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{300}&space;\large&space;M" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\dpi{300}&space;\large&space;M" title="\large M" /></a> <MoA> targets, we make <a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{300}&space;\large&space;N&space;\times&space;M" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\dpi{300}&space;\large&space;N&space;\times&space;M" title="\large N \times M" /></a> predictions. Submissions are scored by the log loss:

<a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{300}&space;\large&space;\text{score}&space;=&space;-&space;\frac{1}{M}\sum_{m=1}^{M}&space;\frac{1}{N}&space;\sum_{i=1}^{N}&space;\left[&space;y_{i,m}&space;\log(\hat{y}_{i,m})&space;&plus;&space;(1&space;-&space;y_{i,m})&space;\log(1&space;-&space;\hat{y}_{i,m})\right]" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\dpi{300}&space;\large&space;\text{score}&space;=&space;-&space;\frac{1}{M}\sum_{m=1}^{M}&space;\frac{1}{N}&space;\sum_{i=1}^{N}&space;\left[&space;y_{i,m}&space;\log(\hat{y}_{i,m})&space;&plus;&space;(1&space;-&space;y_{i,m})&space;\log(1&space;-&space;\hat{y}_{i,m})\right]" title="\large \text{score} = - \frac{1}{M}\sum_{m=1}^{M} \frac{1}{N} \sum_{i=1}^{N} \left[ y_{i,m} \log(\hat{y}_{i,m}) + (1 - y_{i,m}) \log(1 - \hat{y}_{i,m})\right]" /></a>

where:

 - <a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{300}&space;\large&space;N" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\dpi{300}&space;\large&space;N" title="\large N" /></a> is the number of `sig_id` observations in the test data <a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{300}&space;(i=1,&space;...,&space;N)" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\dpi{300}&space;(i=1,&space;...,&space;N)" title="(i=1, ..., N)" /></a>
 
 - <a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{300}&space;\large&space;M" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\dpi{300}&space;\large&space;M" title="\large M" /></a> is the number of scored MoA targets <a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{300}&space;(m=1,&space;...,&space;M)" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\dpi{300}&space;(m=1,&space;...,&space;M)" title="(m=1, ..., M)" /></a>
 
 - <a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{300}&space;\large&space;\hat{y}_{i,m}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\dpi{300}&space;\large&space;\hat{y}_{i,m}" title="\large \hat{y}_{i,m}" /></a> is the predicted probability of a positive `MoA` response for a `sig_id`
 
 - <a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{300}&space;\large&space;y_{i,m}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\dpi{300}&space;\large&space;y_{i,m}" title="\large y_{i,m}" /></a> is the ground truth, 1 for a positive response, 0 otherwise
 
 - <a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{300}&space;\large&space;log()" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\dpi{300}&space;\large&space;log()" title="\large log()" /></a> is the natural (base e) logarithm
 
Note: the actual submitted predicted probabilities are replaced with <a href="https://www.codecogs.com/eqnedit.php?latex=\dpi{300}&space;max(min(p,1-10^{-15}),10^{-15})" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\dpi{300}&space;max(min(p,1-10^{-15}),10^{-15})" title="max(min(p,1-10^{-15}),10^{-15})" /></a>. A smaller log loss is better.

## 2.00 Repository
In this repository, we include the initial Exploratory Data Analysis (EDA) notebook that aims to identify insights to inform our modelling stages. We have also a meta-classifier that classifies so-called 'zero label' records, where a particular record has no label. The results of this meta-classifier are used in the final classifier, in which we train a simple neural network and optimise the architecture in nested cross-validation.

The result of our methodology was positive, but has room for improvement. Pursuing nested-cross validation was incredibly effective at avoiding overfitting. Our first (training validation set), second (out-of-fold validation set) and final test (kaggle's private leaderboard test set) all reported loss scores that were very similar, meaning that the model architecture produced was highly generalisable. However, the loss scores we reported were not as low as other contestants, as we spent a lot of computational time on nested cross-validation meaning less time for experimenting with other model ideas. 

## 3.00 Requirements 

 - Python >= 1.1.4
 - NumPy >= 1.19.4
 - SciPy >= 1.5.4
 - tqdm >= 4.54.1
 - matplotlib >= 3.3.3
 - seaborn >= 0.11.0
 - scikit-learn >= 0.23.2
 - Tensorflow >= 2.3.1
 - scikit-optimize >= 0.8.1
