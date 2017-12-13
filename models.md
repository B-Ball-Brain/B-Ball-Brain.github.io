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

Many of these model have parameter that can be tuned to improve accuracy.  We tuned the alpha parameter of RidgeCV, ElasticNetCV and LassoCV using $$\alpha = 2^x$$ for $$x = -7, \ldots, 4$$.  We used 10 estimators for the ensemble methods as we found that using more estimators causes the models to overfit.

## Classification

We also utilized scikit-learn for binary classification models.  Labels of +1 indicate the plus/minus per minute is nonnegative, and labels of -1 indicate the plus/minus per minute is negative.  We used the following models:

* `LogisticRegression`
* `LinearDiscriminantAnalysis`
* `KNeighborsClassifier`
* `MLPClassifier`
* `GaussianNB`
* `SVC(kernel='linear')`
* `SVC(kernel='rbf')`
* `GradientBoostingClassifier`
* `RandomForestClassifier`

We tuned the ...
