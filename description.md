---
layout: default
title: Description
order: 1
---

## Motivation

Fantasy sports and sports betting are billion dollar industries.  Unsurprisingly, there are huge incentives in predicting the winner of a sports game.  In order to produce accurate predictions of a game’s outcome, many have turned to machine learning.  For example, a maximum entropy principle based algorithm has been used to perform predictions when features are correlated.[^cheng2016predicting] It also compares the results against a number of other classifiers, including ones like Naive Bayes.  These previous works show promising results, but are not perfect.  Events like player injuries, retirements, and player trades can greatly disrupt team dynamics.  A robust model should be able to take into account the chemistry of a team and address questions such as
* How will Team X perform against Team Y?
* A player on Team X is injured.  Who is the best substitute?
* Given a starting lineup of Team Y, which players should Team X start to perform best?

In order tackle these challenging questions, we hope to create a machine learning model that can find deep patterns and connections such as
* A player that is good at assists would complement Team A
* A player that shoots often and passes infrequently clashes with the play style of Team B

## Statistics

Due to the public availability and completeness of the statistics at [stats.nba.com](http://stats.nba.com), we decided to create such a machine learning model for the National Basketball Association (NBA).  The website provides historical player performance statistics such as shooting percentage, number of rebounds, points scored, and number of fouls.  Note that an NBA team can have at most 15 active players, five of which are on the court at a time.  Two of the players are forwards, two are guards, and one is a center.

## Theoretical Description of the Machine Learning Model

An ideal model takes as input the five home team players and five away team players on the court and outputs some metric indicating how well the home team will do compared to the away team.  We determined the most meaningful metric to use is plus/minus per minute.  The plus/minus statistic is the number of points the home team scores minus the points the away team scores.  We then normalize this quantity by time.  Thus, a plus/minus per minute of -4 means the away team outscores the home team by four points every minute.  This statistic is easily accessible from [stats.nba.com](http://stats.nba.com) for use is training and testing.  This can be expressed as

$$
M(P_1, \ldots, P_5, Q_1, \ldots, Q_5) = y,
$$

where $$P_1, \ldots, P_5$$ are players from the home team, $$Q_1, \ldots, Q_5$$ are players from the away team, and $$y$$ is the label of plus/minus per minute.  For consistency, players with subscript 1 are centers, players with subscripts 2 and 3 are forwards, and players with subscripts 4 and 5 are guards.  Note that this is a regression problem.  An alternative approach is to only look at the sign of the plus/minus per minute, which indicates whether or not the home team is winning.  This turns the problem into a simpler classification problem.

Now, we will consider how the players are represented and what information they encode.  We use the extensive player statistics from the 2016-17 season.  Once again, these are averaged per minute for normalization (see [here](http://stats.nba.com/player/2544/traditional/?Season=2016-17&SeasonType=Regular%20Season&PerMode=PerMinute) for example).

$$
P_1, \ldots, P_5, Q_1, \ldots, Q_5 \in \mathbb{R}^d
= \begin{bmatrix}
    \frac{\text{points}}{\text{min}} &
    \text{shooting percentage} &
    \ldots &
    \frac{\text{rebounds}}{\text{min}}    
\end{bmatrix}^\intercal,
$$

where $$d$$ is the number of player statistics.  Thus, we can see the model takes in $$10d$$ input arguments in total.  Through a simple reshaping of the input, we can write the model now as

$$
M(x) = y, \quad x \in \mathbb{R}^{10d}.
$$

## Training Data for the Model

With so many input arguments in the model, a large amount of training data is required.  A single example of training data requires knowing the exact ten players on the court during a portion of a game.  This is paired with a training label, which is the plus/minus per minute during that portion of the game.  Note that within a single game, there are several substitutions made by both teams that change the ten-player matchup on the court.

<img class="main" alt="Game timeline" src="/assets/timeline.svg"/>

Therefore, in a single game, there are numerous "time capsules" and corresponding plus/minus per minute labels that can be used for training.  Moreover, there are 30 NBA teams, each with 82 games per season.  We use all time capsules of at least five minutes from the 2016-17 regular season.  These training examples can be organized into a matrix:

<table class="table table-bordered table-sm">
    <tr>
        <th class="table-secondary" rowspan="3" colspan="2"></th>
        <th class="table-secondary" colspan="3">Season 1</th>
        <th class="table-secondary" colspan="2">Season 2 &hellip;</th>
    </tr>

    <tr>
        <th class="table-secondary" colspan="2">Game 1</th>
        <th class="table-secondary">Game 2 &hellip;</th>
        <th class="table-secondary"></th>
        <th class="table-secondary"></th>
    </tr>

    <tr>
        <th class="table-secondary">Time Cap 1</th>
        <th class="table-secondary">Time Cap 2 &hellip;</th>
        <th class="table-secondary"></th>
        <th class="table-secondary"></th>
        <th class="table-secondary"></th>
    </tr>

    <tr>
        <th class="table-secondary" rowspan="5">Home Team</th>
        <th class="table-secondary">Center</th>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
    </tr>

    <tr>
        <th class="table-secondary">Forward 1</th>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
    </tr>

    <tr>
        <th class="table-secondary">Forward 2</th>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
    </tr>

    <tr>
        <th class="table-secondary">Guard 1</th>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
    </tr>

    <tr>
        <th class="table-secondary">Guard 2</th>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
    </tr>

    <tr>
        <th class="table-secondary" rowspan="5">Away Team</th>
        <th class="table-secondary">Center</th>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
    </tr>

    <tr>
        <th class="table-secondary">Forward 1</th>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
    </tr>

    <tr>
        <th class="table-secondary">Forward 2</th>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
    </tr>

    <tr>
        <th class="table-secondary">Guard 1</th>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
    </tr>

    <tr>
        <th class="table-secondary">Guard 2</th>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
        <td>fgm, reb, blk, &hellip; +/-</td>
    </tr>
</table>

Each cell in the table above is a vector of statistics.  In total, there are 600 rows (features) by 1585 columns (examples).

## Testing Data for the Model

We decided to use the 2017 NBA playoff games for the testing data, as it represents a practical use-case for the model.  There were 79 playoff games and 95 time capsules.

## Using the Model

How does the model address the questions posed in the introduction?  It is easy to see that this model directly answers the question of how Team A will perform compared to Team B via the predicted plus/minus per minute.  How can the model choose an optimal substitute for a team, however?  For each player on the bench capable of playing the position, we can plug their statistics into the model and find which one maximizes the plus/minus per minute.  The problem of finding all five starters to counter the opponent's starting lineup is solved with a similar strategy.  For each position, there is a disjoint set of players capable of playing that position.  We can try all possible team combinations under the position constraints and once again, find the one that maximizes plus/minus per minute.

## References

[^cheng2016predicting]: Cheng, Ge, et al. "Predicting the Outcome of NBA Playoffs Based on the Maximum Entropy Principle." Entropy 18.12 (2016): 450.
