# League of Legends Vision Analysis
this is a project for DSC80 at UCSD

By Xiao Cheng

# Introduction

League of Legends (LoL) is a widely popular multiplayer online battle arena (MOBA) game created by Riot Games. It is one of the most influential and frequently played esports in the gaming industry. The dataset we will be using comes from Oracle’s Elixir and contains detailed match data from professional LoL esports games played throughout 2022.  

This dataset contains important stats and results from a set of LoL games. It provides information about how players perform, how teams work together, and what happens in each match. The data includes statistics that can reflect player actions, team plans, and overall match details.  

In the realm of LoL, the concept of vision refers to what your team can see on the map, which is crucial for making strategic decisions. Since the game map has fog of war (two teams has difference view of map effected by vision), vision helps prevent surprises from the enemy, track their movements, and control key objectives.  

Vision is an important aspect of the game and the game has a built-in way of quantifying vision, the vision score of each player. We will use the vision score data to explore the key question in this project: __In what degree of effectiveness does vision mechanism has to other gaming statistics in the data set__. We want to use data analysis techinques to explore the impact of vision mechanism to on the game play. We want to see whether the red and blue side of the game gets effected by vision score as well as testing hypothesis and exploring predictive approach on top of my finding.

The original data contains 164 columns and 150588 rows, which is representative of 25098 games. Below is an description of columns are that important to this project.

| Column             | Description |
|-------------------|-------------|
| `'gameid'`         | Unique identifier for each match |
| `'side'`           | Team side in the game (Blue or Red) |
| `'result'`         | Match outcome for the team (1 for Win and 0 for Loss) |
| `'gamelength'`     | Total duration of the match in 1234 format, meaning 12 minutes 34 seconds |
| `'visionscore'`    | Vision contribution score for the team or player |
| `'wardsplaced'`    | Number of wards placed during the match |
| `'position'`       | Player’s role/position (Top, Jungle, Mid, ADC, Support) |
| `'killsat15'`      | Number of kills achieved by the player by 15 minutes |
| `'golddiffat15'`   | Gold difference for the player’s team compared to the opponent at 15 minutes |
| `'xpdiffat15'`     | Experience point difference for the player’s team compared to the opponent at 15 minutes |
| `'csdiffat15'`     | Creep score difference for the player compared to the opponent at 15 minutes |
| `'assistsat15'`    | Number of assists by the player by 15 minutes |
| `'deathsat15'`     | Number of deaths for the player by 15 minutes |
| `'golddiffat25'`   | Gold difference for the player’s team compared to the opponent at 25 minutes |

# Data Cleaning and Exploratory Data Analysis

First I identified the important columns to my analysis: 'gameid','side','result','gamelength','visionscore','wardsplaced', 'position','killsat15','golddiffat15', 'xpdiffat15','csdiffat15', 'assistsat15', 'deathsat15','golddiffat25'. I cleaned the data set to contain only the columns that are important to my analysis. In this dataset, each game has 12 rows, with 10 rows representing each of the players, and 2 rows for summarizing the overall team performance and result. I decided to only keep that rows representing players as vision score is a statistic relating to individual player and my analysis does not need the team rows.

Furthermore, I identified that there is one game that has gameid but missing visionscore and wardsplace. Since it is only one, I decided to simply drop that game. Moreover, the column gamelength is storing data in the format 1234 meaning 12 min 34 sec and the data is being stored as integers. I change the data format to be string because 1234 is not intended to mean the number 1234, and I created three new columns, minutes (the number of minutes stored in gamelength), seconds (the number of seconds stored in gamelength), and gamelength_seconds (the total seconds the game took).

Below is the head of my cleand data:

| gameid                  | side | result | gamelength | visionscore | wardsplaced | position | killsat15 | golddiffat15 | xpdiffat15 | csdiffat15 | assistsat15 | deathsat15 | golddiffat25 | minutes | seconds | gamelength_seconds |
|-------------------------|------|--------|------------|-------------|------------|---------|-----------|--------------|------------|------------|-------------|------------|---------------|--------|---------|------------------|
| ESPORTSTMNT01_2690210   | Blue | 0      | 1713       | 26.0        | 8.0        | top     | 0.0       | 391.0        | 345.0      | 14.0       | 1.0         | 0.0        | 605.0         | 17     | 13      | 1033             |
| ESPORTSTMNT01_2690210   | Blue | 0      | 1713       | 48.0        | 6.0        | jng     | 2.0       | 541.0        | -275.0     | -11.0      | 3.0         | 2.0        | 421.0         | 17     | 13      | 1033             |
| ESPORTSTMNT01_2690210   | Blue | 0      | 1713       | 29.0        | 19.0       | mid     | 0.0       | -475.0       | 153.0      | 1.0        | 3.0         | 0.0        | -149.0        | 17     | 13      | 1033             |
| ESPORTSTMNT01_2690210   | Blue | 0      | 1713       | 25.0        | 12.0       | bot     | 2.0       | -793.0       | -1343.0    | -34.0      | 1.0         | 2.0        | -1288.0       | 17     | 13      | 1033             |
| ESPORTSTMNT01_2690210   | Blue | 0      | 1713       | 69.0        | 29.0       | sup     | 1.0       | 443.0        | -497.0     | 7.0        | 2.0         | 2.0        | 499.0         | 17     | 13      | 1033             |

## Exploratory Data Analysis
### Univariate Analysis
In my exploratory data analysis, I first perform univariate analysis to examine the distribution of vision score in my data.
<iframe
  src="assets/visionscore_distr.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The distribution of vision score seems to be right skewed. High vision score is less likely to occur. This is likely because player in a longer game tends to have higher vision score but lone game is rare.

### Bivariate Analysis
I then analyzed the distribution of vision score splitted by each of the five positions.
<iframe
  src="assets/biv_box.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The distribution of vision score is quite different for each position. Supports seems to have higher vision score over because there role is designed to help the team gain vision. Top laners seems to have lower vision score because they are likely to spend more time on the solo lanes rather than playing for the team's vision.

### Grouping and Aggregates
I used a pivot table indexed by side and columns of different positions. The value in this table represents the mean vision score by this position on this side.
| side | bot   | jng   | mid   | sup   | top   |
|------|-------|-------|-------|-------|-------|
| Blue | 38.65 | 43.41 | 33.85 | 79.70 | 31.01 |
| Red  | 37.04 | 42.81 | 33.67 | 78.72 | 30.69 |
This further shows that support and jungle tends to have higher vision score. More interestingly, the blue side tends to have higher vision score compared to the red side. The lets me to question whether the map of the game is unfair, giving the blue side more chances to gain vision. I will examine whether this difference in vision score of the two side is likely due to change or not in the further sections. 

# Assessment of Missingness

## NMAR Analysis
The columns important to this analysis does not included any NMAR values. The columns 'visionscore','wardsplaced', 'position','killsat15','golddiffat15', 'xpdiffat15','csdiffat15', 'assistsat15', 'deathsat15','golddiffat25' all have missing values but all above columns except 'golddiffat25' are missing for the same rows. And the number of rows with each of those columns having missing values is the same. This is indicative that the missingness is related to the game itself rather than the values inside those columns. 'golddiffat25' is missing at random as I will verify with a permutation test next.
Some columns in the original dataset but not related to this project may have NMAR values. For instance, ban1 contains NMAR values because a team can choose to not ban and that will show up as missing in the dataset, so the missingness in the data might mean the value of the ban. Although that ban1 can be missing for other reasons, we are not able to find out the exact reason, but we can conclude that the missingness is related to the missing value itself.

## Missingness Dependency
After analyzing the visionscore, I will examine another statstics that is important to the game play, golddiffat25.
To test missingness dependency, I will focus on the distribution of golddiffat25. I will test this against the columns gamelength and side. 

### Gamelength
First, I examine the distribution of gamelength when golddiffat25 is missing vs not missing.

**Null Hypothesis:** The distribution of gamelength is the same when Duration is missing vs not missing.

**Alternate Hypothesis:** The distribution of gamelength is different when Duration is missing vs not missing

Below is the distribution of gamelength when Duration is missing vs not missing
<iframe
  src="assets/ks_perm.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The two distributions appears do be very different so I will do a permutation test to support the conclusion. I will use KS Statistic to quantify the difference in distribution. I will use a significance level of 0.05.
The observed KS Statistic is 0.31686197914375763, and the p-value is 0.0. This shows that the probability of obtaining this KS Statistic under the null hypothesis is 0. Therefore, I concluded that I have convincing evdience that the distribution of gamelength when golddiffat25 is missing is different from the distribution of gamelength when golddiffat25 is not missing. Therefore, the missingess of golddiffat25 is dependent on the gamelength. This is intuitive because games that did not last 25 min will not have a golddiffat25.


### Gamelength
Next, I examine whether missingess golddiffat25 is related to side.

**Null Hypothesis:** The distribution of side is the same when Duration is missing vs not missing.

**Alternate Hypothesis:** The distribution of side is different when Duration is missing vs not missing

I used TVD as the test statistic and run a permutation test under 0.05 significance level.
I got the observed TVD being 0 and a p-value of 1. This shows that the number of rows with side being blue equals the number of row with side being red regardless of whether the golddiffat25 is missing. Therefore, I concluded that the missingness of golddiffat25 is not dependent of side.

# Hypothesis Testing
I will then explore whether the difference in visionscore when splitted by side discovered earlier is purely due to change or not.

**Null Hypothesis**: The distribution of vision score for the blue side is the same as the team on the red side
**Alternative Hypothesis**: The distribution of vision score for the blue side is NOT the same as the team of the red side.
I will use absolute difference in mean as my test statistic because it is simple to compute. I significant difference in mean could lead to rejection of null hypopthesis without needing to use a more complicated test statistic such as the KS statistic. I run a permutation test with significance level 0.05.
Below is the distribution of absolute difference in mean under the null hypothesis and the line indicats the observed difference in mean.
<iframe
  src="assets/vision_perm.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
I got an observed absolute difference of 0.7396238444373608 and a p-value of 0.0. Therefore, I reject and null hypothesis and claim that the distribution of vision score is different depending on the side. Therefore, it is possible that map of LoL give some advantage in terms of vision to one side or an other. Using the pivot table earlier, blue side might be benefitting.

# Framing a Prediction Problem
I am going to predict the 'golddiffat25' column which is the difference in gold of the two teams at 25 minutes into the game. This is a regression problem. I will try to perform the prediction at 15 minuites into the game because using data after 25 minutes to predict statistics of 25 minutes is useless, so I try to make predictions earlier and while having enough data the can be representative of the game.
The metric I will use to assess my model will be root mean squared error (RMSE) because it is representative of the model's performance. It can be used to assess performance on unseen data. I will also use R^2 in the report because it is more interpretable. 

# Baseline Model
The baseline model I built is linear regression with two features, golddiffat15 and side. golddiffat15 is quantitative feature and side is nomial feature. I used one hot encoding to transform side. I used sklearn's library to do one hot encoding and I also dropped one column to avoid colinearity. 
I used a 80-20 train test split to assess my model.
The test RMSE is 1178.58 and the R^2 is 0.635. I believe this model is doing well as the R^2 shows it is capturing 63.5% of the variance in the response variable. I think that the game has a lot of variability to it so it will be extremely diffult to predict middle and late game performance based on the early game. One team fight could change the difference in gold completely. Therefore, there is an extremely high variance in the underlying data generating process. Therefore, while 0.635 is not very high, I believe it is close to the theotical best.

# Final Model
I used a ridge regression, and I used GridSearch CV to find the optimal shrinkage parameter. I used ridge because I the stats avaliable at 15 minutes are likely to have high collinearity, so I will try to penalize that. I also tried to do polynomial regression as I suspect that the golddiffat15 and golddiffat25 might not be linearly related because the team with advantages tends to gain gold faster than the opponent. I added "position", "xpdiffat15", "csdiffat15", "killsat15", "assistsat15" because they are the only stats avaliable at 15 that seems to be representative of the game. "deathsat15" is very close to "killsat15" because both side of the game is in the data, so I did not use it. I believe those features can provide a better summary of the game play. The gold at 15 might be achieve with the loss of those other stats and adding them takes this into account. 
The best shrinkage paramter is alpha: 1389.4954943731361
The test RMSE is: 1172.096812383099
The best model is with degree 3 polynomial
The test RMSE decrease by a little, showing some improvement

# Fairness Analysis
I tested whether my model is performing well on player with at least one death in the first 15 minutes and those did not. I use a permutation test and a test statistic of difference of rmse. 
**Null Hypothesis:** The distribution of rmse is the same for players with at least one death within 15 minutes and those did not die.

**Alternate Hypothesis:** The distribution of rmse is different for players with at least one death within 15 minutes and those did not die.
Observed RMSE difference (Died - Did not Die): 23.9977378229039
Permutation p-value: 0.0
I conclude that my model is being unfair to those have died at least once. The model is performing worse for player with at least one death within 15 minutes.

