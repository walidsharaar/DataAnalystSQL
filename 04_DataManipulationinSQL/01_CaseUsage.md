## Summary We'll take the CASE | SQL
Intermediate SQL, focusing on shaping and manipulating data using CASE statements, subqueries, and window functions within the context of the European Soccer Database.

### Facts
- Intermediate SQL provides skills to access and shape robust datasets in relational databases.
- Covered topics include CASE statements, subqueries, and window functions for data transformation.
- Prerequisites involve familiarity with basic SQL concepts like joins, arithmetic functions, and filtering with WHERE clauses.
- The course employs the European Soccer Database with tables for matches, teams, leagues, and countries.
- Analyzing wins, losses, and ties for the 2013/2014 season involves using the "Match" table's home_goal and away_goal columns.
- CASE statements act as SQL's conditional statements, allowing for conditional data manipulation within queries.
- Example: Creating new variables to categorize match outcomes (home team wins, away team wins, or ties) using CASE statements.
- Practice in upcoming lessons involves structuring CASE statements with arithmetic functions (COUNT, SUM, AVERAGE) to categorize data.
- 
```
--Select the team's long name and API id from the teams_germany table. Filter the query for FC Schalke 04 and FC Bayern Munich using IN, giving you the team_api_IDs needed for the next step.
SELECT
	-- Select the team long name and team API id
	team_long_name,
	team_api_id
FROM teams_germany
-- Only include FC Schalke 04 and FC Bayern Munich
WHERE team_long_name in ('FC Schalke 04', 'FC Bayern Munich');

-- Identify the home team as Bayern Munich, Schalke 04, or neither
SELECT 
    CASE WHEN hometeam_id = 10189 THEN 'FC Schalke 04'
         WHEN hometeam_id = 9823 THEN 'FC Bayern Munich'
         ELSE 'Other' END AS home_team,
	COUNT(id) AS total_matches
FROM matches_germany
-- Group by the CASE statement alias
GROUP BY home_team;

--Select the date of the match and create a CASE statement to identify matches as home wins, home losses, or ties.
SELECT 
	-- Select the date of the match
	date,
	-- Identify home wins, losses, or ties
	CASE WHEN home_goal > away_goal THEN 'Home win!'
         WHEN home_goal < away_goal THEN 'Home loss :(' 
         ELSE 'Tie' END AS outcome
FROM matches_spain;

--Filter for matches where the home team is FC Barcelona (id = 8634)
SELECT 
	m.date,
	t.team_long_name AS opponent,
	-- Complete the CASE statement with an alias
	CASE WHEN m.home_goal > m.away_goal THEN 'Barcelona win!'
         WHEN m.home_goal < m.away_goal THEN 'Barcelona loss :(' 
         ELSE 'Tie' END AS outcome 
FROM matches_spain AS m
LEFT JOIN teams_spain AS t 
ON m.awayteam_id = t.team_api_id
-- Filter for Barcelona as the home team
WHERE m.hometeam_id = 8634; 
```
