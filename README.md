# SQL-Cricket-Match-Data-Analysis-Project
# Introduction
Cricket is one of the most popular sports worldwide, and analyzing cricket match data can uncover valuable insights into player performance, team strategies, and game dynamics. This project aims to explore a cricket dataset to identify patterns, trends, and statistics that can improve understanding and decision-making in the game.

The focus areas include descriptive statistics, performance analysis, situational insights, player-specific metrics, team dynamics, game situations, and comparative analysis. These analyses will help answer critical questions about the game and its players.

#  Data Overview
The dataset contains detailed information about cricket matches, including:

Match Details: Match ID, innings, overs, ball number, and batting team.
Player Information: Batter, bowler, and non-striker.
Run Details: Batsman runs, extras, total runs, and boundary indicators.
Wickets and Fielding: Wicket deliveries, players dismissed, types of dismissals, and fielders involved.
Extras: Types of extras (e.g., wides, no-balls).
Analysis Questions and SQL Solutions

###open the table;
SELECT * FROM ipl_data_analysis.ipl_ball_by_ball_2008_2022;

##Descriptive Statistics
### 1. What are the total runs scored by each team?
SELECT 
    BattingTeam, SUM(total_run) AS Total_runs
FROM
    ipl_ball_by_ball_2008_2022
GROUP BY BattingTeam
ORDER BY Total_runs DESC
LIMIT 10;

### 2.Which batter scored the most runs overall?
SELECT 
    batter, SUM(total_run) AS Batsman_Run
FROM
    ipl_ball_by_ball_2008_2022
GROUP BY batter
ORDER BY batsman_run DESC
LIMIT 1;

### 3.Which bowler conceded the most runs?
SELECT 
    bowler, SUM(total_run) Run_conceded
FROM
    ipl_ball_by_ball_2008_2022
GROUP BY bowler
ORDER BY Run_conceded DESC
LIMIT 1;

### 4.What is the strike rate of each batter?
SELECT 
    batter,
    ROUND((SUM(batsman_run) / SUM(ballnumber) * 100),
            2) AS Strick_Rate
FROM
    ipl_ball_by_ball_2008_2022
GROUP BY batter
ORDER BY Strick_Rate DESC;

### 5.What is the economy rate of each bowler?
SELECT 
    bowler,
    ROUND(SUM(total_run) / SUM(overs), 2) AS Economy_rate
FROM
    ipl_ball_by_ball_2008_2022
GROUP BY bowler
ORDER BY Economy_rate DESC;

### 6.Which bowler took the most wickets, and against which team?
SELECT 
    bowler,
    SUM(isWicketDelivery) AS Took_wickets,
    BattingTeam AS oponent
FROM
    ipl_ball_by_ball_2008_2022
GROUP BY bowler , oponent
ORDER BY Took_wickets DESC;

###Situational Insights
### 7.Which over(s) yielded the most runs for a team?
SELECT 
    BattingTeam, overs, SUM(total_run) AS runs_in_over
FROM
    ipl_ball_by_ball_2008_2022
GROUP BY BattingTeam , overs
ORDER BY runs_in_over DESC
LIMIT 5;

### 8.During which innings are more wickets typically lost?

SELECT 
    innings, COUNT(*) AS wickets_lost
FROM
    ipl_ball_by_ball_2008_2022
WHERE
    isWicketDelivery = 1
GROUP BY innings
ORDER BY wickets_lost DESC;

### 9.Which bowler performed best under pressure (e.g., last overs)?

SELECT 
    bowler,
    SUM(total_run) AS runs_conceded,
    ROUND(SUM(total_run) / COUNT(*) * 6, 2) AS economy_rate,
    SUM(CASE
        WHEN isWicketDelivery = 1 THEN 1
        ELSE 0
    END) AS wickets_taken
FROM
    ipl_ball_by_ball_2008_2022
WHERE
    overs BETWEEN 16 AND 20
GROUP BY bowler
ORDER BY economy_rate ASC , wickets_taken DESC
LIMIT 10;

##Player-Specific Analysis
## 10.Which batter has the highest average runs per game?

SELECT 
    batter,
    SUM(batsman_run) AS total_runs,
    COUNT(DISTINCT ID) AS matches,
    ROUND(SUM(batsman_run) / COUNT(DISTINCT ID), 2) AS average_runs
FROM
    ipl_ball_by_ball_2008_2022
GROUP BY batter
ORDER BY average_runs DESC
LIMIT 1;

### 11.Which players are most frequently involved in partnerships as a non-striker?

SELECT 
    `non-striker` AS player,
    COUNT(*) AS total_partnership_involvements
FROM 
    ipl_ball_by_ball_2008_2022
GROUP BY 
    `non-striker`
ORDER BY 
    total_partnership_involvements DESC;


## 12.How often does each batter get dismissed by specific bowlers?

SELECT 
    batter, bowler, COUNT(*) AS dismissals
FROM
    ipl_ball_by_ball_2008_2022
WHERE
    isWicketDelivery = 1
GROUP BY batter , bowler
ORDER BY dismissals DESC;

##Team Dynamics
## 13.Which team has the best average score when batting first?

SELECT 
    BattingTeam, ROUND(AVG(total_run), 2) AS average_score
FROM
    ipl_ball_by_ball_2008_2022
WHERE
    innings = 1
GROUP BY BattingTeam
ORDER BY average_score DESC;

## 14.How effective are fielders from each team in contributing to wickets?

SELECT 
    fielders_involved, COUNT(*) AS wickets_contributed
FROM
    ipl_ball_by_ball_2008_2022
WHERE
    isWicketDelivery = 1
        AND fielders_involved IS NOT NULL
GROUP BY fielders_involved
ORDER BY wickets_contributed DESC;

## Conclusion
This project analyzed cricket data to uncover key performance trends for teams and players. The results can help teams optimize strategies and identify players who perform well under different circumstances.


  
