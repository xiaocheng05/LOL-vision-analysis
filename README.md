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

First I identified the important columns to my analysis: 'gameid','side','result','gamelength','visionscore','wardsplaced', 'position','killsat15','golddiffat15', 'xpdiffat15','csdiffat15', 'assistsat15', 'deathsat15','golddiffat25'. I cleaned the data set to contain only the columns that are important to my analysis. 
