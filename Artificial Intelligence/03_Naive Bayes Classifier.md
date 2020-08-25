# Naive Bayes Classifier

## Features

* We use features to simplify the classification and clustering problems
* Features turn objects (images, documents, etc) into a set of numbers so that we can do computation over them
* What features can we use for the image classification? For spam email classification?



## Problems

* Digit Recognition
* Email spam filtering
* How would you do this?



## Classification

### Simple Classification

* Simple example: two binary features
  ![{\displaystyle {\mbox{posterior}}={\frac {{\mbox{prior}}\times {\mbox{likelihood}}}{\mbox{evidence}}}.\,}](https://wikimedia.org/api/rest_v1/media/math/render/svg/679e25db34f602d562e503af6d772125f78ab31e)

### Digit Recognizer

* input: pixel grids (8x8=64 features)
* output: a digit 0-9

### Naive Bayes for Digits

* Simple version
* Naive Bayes model:
  ![{\displaystyle p(C_{k}\vert x_{1},\dots ,x_{n})={\frac {1}{Z}}p(C_{k})\prod _{i=1}^{n}p(x_{i}\vert C_{k})}](https://wikimedia.org/api/rest_v1/media/math/render/svg/9dd841d7c36e6d7449bea439ef99e8138810870d)

* Example: Conditional Probability Table(CPTs)
  * 각각의 pixel이 black or white(on or off)인지의 확률
* Training data

### Parameter Estimation

* Estimating distribution of random variables like X or X|Y
* **Empirically**: user training data
  * For each outcome x, look at the **empirical** rate of that value
* **Elicitation**: ask a human
  * Usually need domain experts

### Naive Bayes for Text

* Bag-of-Words Naive Bayes
  * frequency

### Overfitting

* Relative frequency parameters will **overfit** the training data

  * ex. 3을 적은 다음 .을 찍는 경우

* As an extreme case, imagine using the entire email as the only feature

* To generalize better: we need to **smooth** or regularize the estimates

  

## Estimation: Smoothing

=regularization

* Problems with maximum likelihood estimates
* Basic idea:
  * We have some prior expectation about parameters (here, the probability of heads)
  * Given little evidence, we should skew towards our prior
    * p(x)+k / sample+k
  * Given a **lot of evidence**, we should **listen to the data**

### Laplace smoothing

* Laplace's estimate
* Pretend you saw every outcome once more than you actually did



## Tuning on Held-Out Data

* **Hyperparameters**
* Where to learn?
  * **Validation data (=held-out data)**
  * Learn parameters from training data
  * Must tune hyperparameters on different data
  * For each value of the hyperparameters, train and test on the held-out data
  * Choose the best value and do a final test on the test data
  * Dataset
    * 8 : 1 : 1 = training : validtaion : test



## Baselines

### First task: get a baseline

* Baselines are very simple "straw man" procedures
* Help determine how hard the task is
* Help know what a "good" accuracy is

### Weak baseline: most frequent label classifier

* Gives all test instances whatever label was most common in the training set
* E.g. for sapm filtering, might label everthing as ham
* Accuracy might be very high if the problem is skewed

### For real research, usually use previous work as a (strong) baseline



## What to do about Errors

* Problem: there's still spam in your inbox
* Need **more features**-words aren't enough
* Naive Bayes models can incorporate a variety of features, but tend to do best in homogeneous cases (e.g. all features are word occurrences)



## Features

* A feature is a function that signals a property of the input
  * Naive Bayes: features are random variables & each value has conditional **probabilities** given the label
  * Most classifiers: featuers are **real-valued** functions
  * Common special cases:
    * <u>Indicator features</u> take values 0 and 1 or -1 and 1
    * <u>Count features</u> return non-negative integers

* Features are anything you can think of for which you can write code to evaluate on an input
  * Many are cheap, but some are expensive to compute
  * Can even be the output of another classifier or model
  * Domain knowledge goes here



## Summary

* Bayes rule lets us do diagnotic queries with **conditional probabilities**
* The Naive Bayes assumption takes **all features** to be independent given the class label
* We can build classifiers out of a Naive Bayes model using trining data
* **Smoothing** estimates is important in real systems
  * = generalization (regularization보단 generalization)



### 참고

* 오일석, '패턴인식', 교보문고(2016)