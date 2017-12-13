---
layout: default
title: Results
order: 3
---

## Regression

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

## Comparison to Previous Work

Torres et. al. and Loeffelholz et. all both attempted to predict results of NBA games using machine learning. They used box scores of teams from games played earlier in the season to train their models then they used the trained models to make predictions about the remaining games in the season. Below is a table showing the results from both experiments:

<table class="table table-bordered table-sm">
    <tr>
    	<th class="table-secondary" colspan="1">Models</th>
        <th class="table-secondary" rowspan="1" colspan="1">Average Results from Validation set</th>
    </tr>
    <tr>
    	<th class="table-secondary" colspan="1">FFNN</th>
        <td>71.67</td>
    </tr>
    <tr>
    	<th class="table-secondary" colspan="1">RBF</th>
        <td>68.67</td>
    </tr>
    <tr>
    	<th class="table-secondary" colspan="1">PNN</th>
        <td>71.33</td>
    </tr>
    <tr>
    	<th class="table-secondary" colspan="1">GRNN</th>
        <td>71.33</td>
    </tr>
    <tr>
    	<th class="table-secondary" colspan="1">PNN Fusion</th>
        <td>71.67</td>
    </tr>
    <tr>
    	<th class="table-secondary" colspan="1">Bayes Fusion</th>
        <td>71.67</td>
    </tr>
    </table>

    Results from Loeffelholz et. al. showing predictions for 30 games in 2007 - 2008 NBA Season.

<table class="table table-bordered table-sm">
    <tr>
    	<th class="table-secondary" colspan="1">Models</th>
        <th class="table-secondary" rowspan="1" colspan="1">Average Results from Validation set</th>
    </tr>
    <tr>
    	<th class="table-secondary" colspan="1">Linear Regression</th>
        <td>0.6991</td>
    </tr>
    <tr>
    	<th class="table-secondary" colspan="1">Logistic Regression</th>
        <td>0.6744</td>
    </tr>
    <tr>
    	<th class="table-secondary" colspan="1">SVM</th>
        <td>0.6596</td>
    </tr>
    <tr>
    	<th class="table-secondary" colspan="1">ANN</th>
        <td>0.6478</td>
    </tr>
   
    </table>

    Results from Torres et. al. showing mean of predictions for NBA games between 1992 - 1996.
	

Comparing the results from the tables above with the results from our model we can see that our models do a descent job of making predictions given that we are solving a harder problem of predicting results given player mash-ups rather than learning team performance over different games and seasons. Also, our model tries to select the best players to give a team the best chance of winning and it learns by examining player stats from a 30 seconds time capsule over for all the games played in a season. This presents a significant challenge even for a human expert.