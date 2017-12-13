---
layout: default
title: Models
order: 2
---

## Regression

For this project, we first attempted to solve the regression form of the problem.  We used [scikit-learn](http://scikit-learn.org), a machine learning library written in Python, to accomplish this task. The models we used were:

* `LinearRegression`
* `BayesianRidgeRegression`
* `RidgeCVRegression`
* `ElasticNetCVRegression`
* `LassoCVRegression`
* `GradientBoostingRegressor`
* `AdaBoostRegressor`
* `ExtraTreesRegressor`
* `RandomForestRegressor`
* `LinearSVR`

Many of these model have parameter that can be tuned to improve accuracy.  We tuned the alpha parameter of RidgeCV, ElasticNetCV and LassoCV using $$\alpha = 2^x$$ for $$x = -7, \ldots, 4$$.  We used 10 estimators for the ensemble methods as we found that using more estimators causes the models to overfit.

## Classification

We also utilized scikit-learn for binary classification models.  Labels of +1 indicate the plus/minus per minute is nonnegative, and labels of -1 indicate the plus/minus per minute is negative.  We used the following models:

* `LogisticRegression`
* `LinearDiscriminantAnalysis`
* `GaussianNB`
* `GradientBoostingClassifier`
* `RandomForestClassifier`
* A Feedforward Neural Network (`FNN`) in [Keras](http://keras.io) using Dropout regularization

We tuned the following classifiers: `LogisticRegression`, `GradientBoosting` and `FNN` to achieve ~60% accuracy in testing data while not over-fitting the models. For the `LogisticRegression` classifier, we set the constant, C, that multiplies the L2 regularization term to `C = 0.0005`. We set the number of estimators in GradientBoosting to 400 and the learning rate to 0.01. We tried multiple other values for these parameters both small and big before we narrowed down on these values.

The Feedforward Neural Network has one hidden layer with 50 neurons, uses sigmoid as the activation function at both the hidden and the output layer. We use dropouts with `p = 0.5` at both the input and hidden layer.
