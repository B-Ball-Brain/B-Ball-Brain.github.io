---
layout: default
title: Results
order: 3
---

## Regression

<table class="table table-bordered table-sm">
    <tr>
    	<th class="table-secondary" colspan="1">Models</th>
        <th class="table-secondary" rowspan="1" colspan="1">Duration</th>
         <th class="table-secondary" rowspan="1" colspan="1">TrainMSE</th>
          <th class="table-secondary" rowspan="1" colspan="1">TestMSE</th>
           <th class="table-secondary" rowspan="1" colspan="1">R^2 Score</th>
    </tr>
    <tr>
    	<td class="table-secondary" colspan="1">DummyRegressor</td>
        <td>0.0003519058</td>
        <td>4.292656363</td>
        <td>4.012747466</td>
        <td>-8.05E-06</td>
    </tr>
    <tr>
    	<td class="table-secondary" colspan="1">LinearRegression</td>
        <td>2.876500845</td>
        <td>4.168436111</td>
        <td>4.03817326</td>  
        <td>-0.0063443617</td>
    </tr>
    <tr>
    	<td class="table-secondary" colspan="1">BayesianRidge</td>
        <td>9.113944054</td>
        <td>4.240433111</td>  
        <td>3.978284211</td>
        <td>0.0085804578</td>
    </tr>
    <tr>
    	<td class="table-secondary" colspan="1">RidgeCV</td>
        <td>14.15430808</td>
        <td>4.167578924</td>
        <td>4.03089686</td>
        <td>-0.0045310258</td>
    </tr>
    <tr>
    	<td class="table-secondary" colspan="1">ElasticNetCV</td>
        <td>6.157432079</td>
        <td>4.24524836</td>
        <td>3.981448748</td>
        <td>0.0077918304</td>
    </tr>
    <tr>
    	<td class="table-secondary" colspan="1">LassoCV</td>
        <td>6.049508095</td>
        <td>4.244920397</td>
        <td>3.981493739</td>
        <td>0.0077806182</td>
    </tr>
    <tr>
    	<td class="table-secondary" colspan="1">GradientBoostingRegressor</td>
        <td>10.3492372</td>
        <td>4.250431632</td>
        <td>3.994107012</td>
        <td>0.004637292</td>
    </tr>
    <tr>
    	<td class="table-secondary" colspan="1">AdaBoostRegressor</td>
        <td>32.49189615</td>
        <td>4.263147054</td>
        <td>3.994472842</td>
        <td>0.0045461242</td>
    </tr>
    <tr>
    	<td class="table-secondary" colspan="1">ExtraTreesRegressor</td>
        <td>70.33536506</td>
        <td>0.2614326469</td>
        <td>4.934085332</td>
        <td>-0.2296126575</td>
    </tr>
    <tr>
    	<td class="table-secondary" colspan="1">RandomForestRegressor</td>
        <td>135.7508721</td>
        <td>1.08187359</td>
        <td>4.861826425</td>
        <td>-0.2116051728</td>
    </tr>
    <tr>
    	<td class="table-secondary" colspan="1">LinearSVR</td>
        <td>53.98221612</td>
        <td>4.314991285</td>
        <td>4.073052891</td>
        <td>-0.0150366387</td>
    </tr>   
</table>

    Results from our regression models for the play-offs games.

## Examining the Data

The poor results of the regression models can be attributed to issues in the models or data.  A closer examination of the data indicates the latter due to a substantial amount of noise.

<div class="row">
    <div class="col-md-6">
        <img class="img-fluid" src="/assets/win-percentage.png"/>
    </div>

    <div class="col-md-6">
        <img class="img-fluid" src="/assets/plus-minus.png"/>
    </div>
</div>

The scatter plots above help demonstrate this noise.  Each point represents a time capsule, with the x-position indicating the plus-minus per minute for that capsule.  For the y-position, we take the training data (which uses season-averaged player statistic), average the five home and five away team players' statistics for each time capsule, and compute the difference in a statistic.  We would expect teams with a high win percentage to outscore opponents with a low win percentage.  From the best fit line, we can see this is roughly true, but the correlation between plus/minus per minute and win percentage is almost zero.  A similar situation occurs when we compare time capsule plus/minus per minute to season-averaged plus/minus per minute.

There are several reasons we have hypothesized as to why this noise is present

* Time capsules are as short as 30 seconds.  This does not allow must time for scoring to "smooth out" like it would over an entire game.
* Scoring streaks cause a large number of outliers.
* If a team is winning by a large margin at the end of a game, they may allow opponents to score more since it will not affect the game outcome.

These results suggest that a classification model might perform better, as it simplifies the problem.

## Classification

<table class="table table-bordered table-sm">
    <tr>
    	<th class="table-secondary" colspan="1">Models</th>
        <th class="table-secondary" rowspan="1" colspan="1">Duration</th>
         <th class="table-secondary" rowspan="1" colspan="1">Train Accuracy</th>
          <th class="table-secondary" rowspan="1" colspan="1">Test Accuracy</th>
    </tr>
    <tr>
    	<td class="table-secondary" colspan="1">DummyClassifier</td>
        <td>0.001994133</td>
        <td>52.14%</td>
        <td>49.71%</td>
    </tr>
    <tr>
    	<td class="table-secondary" colspan="1">LogisticRegression</td>
        <td>8.462990046</td>
        <td>60.50%</td>
        <td>60.58%</td>
    </tr>
    <tr>
    	<td class="table-secondary" colspan="1">LinearDiscriminantAnalysis</td>
        <td>6.796962977</td>
        <td>61.12%</td>
        <td>58.98%</td>
    </tr>
    <tr>
    	<td class="table-secondary" colspan="1">GaussianNB</td>
        <td>0.6752369404</td>
        <td>53.71%</td>
        <td>53.22%</td>
    </tr>
    <tr>
    	<td class="table-secondary" colspan="1">GradientBoostingClassifier</td>
        <td>147.259727</td>
        <td>60.08%</td>
        <td>60.31%</td>
    </tr>
    <tr>
    	<td class="table-secondary" colspan="1">RandomForestClassifier</td>
        <td>5.883100033</td>
        <td>94.39%</td>
        <td>51.62%</td>
    </tr>
    <tr>
    	<td class="table-secondary" colspan="1">FNN with Dropout Reg</td>
        <td>46.352</td>
        <td>60.65%</td>
        <td>60.36%</td>
    </tr>
    </table>

    Results from our classification models for the play-offs games.

## Comparison to Previous Work

Torres et. al. and Loeffelholz et. al. both attempted to predict results of NBA games using machine learning.[^loeffelholz2009predicting] [^torres2013prediction] They used box scores of teams from games played earlier in the season to train their models then they used the trained models to make predictions about the remaining games in the season. Below is a table showing the results from both experiments:

<table class="table table-bordered table-sm">
    <tr>
        <th class="table-secondary">Source</th>
    	<th class="table-secondary">Model</th>
        <th class="table-secondary">Average Classification Accuracy</th>
    </tr>
    <tr>
        <td rowspan="6">Loeffelholz et. al.</td>
    	<td>FFNN</td>
        <td>71.67%</td>
    </tr>
    <tr>
    	<td>RBF</td>
        <td>68.67%</td>
    </tr>
    <tr>
    	<td>PNN</td>
        <td>71.33%</td>
    </tr>
    <tr>
    	<td>GRNN</td>
        <td>71.33%</td>
    </tr>
    <tr>
    	<td>PNN Fusion</td>
        <td>71.67%</td>
    </tr>
    <tr>
    	<td>Bayes Fusion</td>
        <td>71.67%</td>
    </tr>
    <tr>
        <td rowspan="4">Torres et. al.</td>
    	<td>Linear Regression</td>
        <td>69.91%</td>
    </tr>
    <tr>
    	<td>Logistic Regression</td>
        <td>67.44%</td>
    </tr>
    <tr>
    	<td>SVM</td>
        <td>65.96%</td>
    </tr>
    <tr>
    	<td>ANN</td>
        <td>64.78%</td>
    </tr>
</table>

Comparing the results from the table above with the results from our models, we can see that our models do a descent job of making predictions given that we are solving a harder problem of predicting results given player matchups rather than learning team performance over different games and seasons. Also, our model tries to select the best players to give a team the best chance of winning, and it learns by examining player statistics from a 30-second time capsules over a season. This presents a significant challenge even for a human expert.

## References
[^loeffelholz2009predicting]: Loeffelholz, Bernard, Earl Bednar, and Kenneth W. Bauer. "Predicting NBA games using neural networks." Journal of Quantitative Analysis in Sports 5.1 (2009).
[^torres2013prediction]: Torres, Renator Amorim. "Prediction of nba games based on machine learning methods." University of Wisconsin, Madison (2013).
