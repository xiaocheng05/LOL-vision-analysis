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

### Bivariate Analysis
I then analyzed the distribution of vision score splitted by each of the five positions.
<iframe
  src="assets/biv_box.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


