---
layout: default
title: Models
order: 2
---

## Models

For this project we attempted to solve a regression problem by training models to predict scores from player mash-ups. We used Scikit-learn, a machine learning library written in python to accomplish this task. We trained our data on the following models:

* Linear Regression
* Bayesian Ridge Regression
* RidgeCV Regression
* ElasticNetCV Regression
* LassoCV Regression
* GradientBoostingRegressor
* AdaBoostRegressor
* ExtraTreesRegressor
* RandomForestRegressor

We tuned the alpha parameter $$x$$ of RidgeCV, ElasticNetCV and LassoCV using $$2^x \quad x \in range \numrange{-7}{4}$$, also we used 10 estimators for the ensemble methods as we found that using more estimators makes the model to overfit.

After we got results (described in the analysis page) from our models we transformed the labels of our data to +1 or -1 in order to run binary classification algorithms on our data. We used the following models in training our data: 

* LogisticRegression
* LinearDiscriminantAnalysis
* KNeighborsClassifier
* MLPClassifier
* GaussianNB
* SVC(kernel='linear')
* SVC(kernel='rbf')
* GradientBoostingClassifier
* RandomForestClassifier

We tuned the ...