# PU-learning-example
An example repo for how PU Bagging and TSA works. 

**In a nutshell:** You have a lot of unlabelled or unreliable negative samples and very few postively labelled samples. You want to be able to train on positive and negatives. In order to get around this you can use "PUBagging" or the "two step approach".


### PU Bagging
- Randomly sample all positives and a subset of unlaballed data
- Build a classifier with this bootstrapped dataset -> using positive as 1 and unknown as 0 
- Predict classes of the unknowns that were not sampled in training (known as Out-of-bag samples)
- Repeat many times and get the OOB average scores

**<hr>**

### Two Way Approach for PU Learning 

- Identify a subset of of the data that can be confidentally labelled as negative (reliable negatives)
- Use reliable negatives and positives on a classifier and use that to label your unknown samples. 

**But it's not always the case that you have reliable negative cases in your dataset.. You only have positive and unknown.** <br>
To mitigate this you need to :
1. Train a RF on Positive and unlaballed cases.  Make a score range using rf.predict_proba() function and make a range from lowest score found for positive case to highest score found for positive case. Relabelled the data set as Positive/negative with that score range.
2. Train a second RF on the newly labelled data 

<hr>

### Data
For this example i will be using the **Banknote Dataset.** 

Negatives: 762
Positives: 610
<br>

**Description**
The Banknote Dataset involves predicting whether a given banknote is authentic given a number of measures taken from a photograph.
It is a binary (2-class) classification problem. The number of observations for each class is not balanced. There are 1,372 observations with 4 input variables and 1 output variable. The variable names are as follows:

- Variance of Wavelet Transformed image (continuous).
- Skewness of Wavelet Transformed image (continuous).
- Kurtosis of Wavelet Transformed image (continuous).
- Entropy of image (continuous).
- Class (0 for authentic, 1 for inauthentic).

The baseline performance of predicting the most prevalent class is a classification accuracy of approximately 50%.

##### Source
- http://archive.ics.uci.edu/ml/datasets/banknote+authentication


#### Useful Notes
- PUBagging is quicker than normal ensemble methods (with tree based algorithms) as it makes use of parallelization 
- Two step approach provides better accuracy for Tree based alogorithms
- PUBagging provides the best results overall with SVM's, BUT it takes incredibly long to train. 
- https://roywright.me/2017/11/16/positive-unlabeled-learning/