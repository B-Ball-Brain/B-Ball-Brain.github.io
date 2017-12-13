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
