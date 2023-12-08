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

## Summary In CASE things get more complex | SQL

Understanding CASE statements involves testing logical conditions to categorize data into different outcomes. Handling complex tests, including multiple logical conditions, and managing NULL values are vital aspects of utilizing CASE statements effectively.

### Facts
- CASE statements expand into more complex logical tests beyond basic conditions.
- Review of CASE WHEN: One logical test per WHEN statement, categorizing outcomes based on conditions (e.g., higher scores for home/away teams).
- CASE WHEN ... AND logic: Multiple logical conditions in a WHEN clause using AND to differentiate outcomes (e.g., identifying matches won by Chelsea, both home and away).
- Importance of ELSE: Consideration of what data is included in the ELSE clause, categorizing unmatched rows.
- Correct categorization: Filtering data with specific conditions in the WHERE clause to accurately classify outcomes.
- Dealing with NULL values: Understanding the impact of ELSE NULL and handling NULL values in query results.
- Filtering with CASE: Utilizing CASE statements as filter criteria in the WHERE clause to refine data retrieval.
- Optimal placement of CASE: Leveraging CASE statements in the WHERE clause to filter for specific outcomes without additional conditions.
  
```
--Complete the first CASE statement, identifying Barcelona or Real Madrid as the home team using the hometeam_id column.
SELECT 
	date,
	-- Identify the home team as Barcelona or Real Madrid
	CASE WHEN hometeam_id = 8634 THEN 'FC Barcelona' 
         ELSE 'Real Madrid CF' END AS home,
    -- Identify the away team as Barcelona or Real Madrid
	CASE WHEN awayteam_id = 8634 THEN 'FC Barcelona' 
         ELSE 'Real Madrid CF' END AS away
FROM matches_spain
WHERE (awayteam_id = 8634 OR hometeam_id = 8634)
      AND (awayteam_id = 8633 OR hometeam_id = 8633);

--Identify Bologna's team ID listed in the teams_italy table by selecting the team_long_name and team_api_id.
-- Select team_long_name and team_api_id from team
SELECT
	team_long_name,
	team_api_id
FROM teams_italy
-- Filter for team long name
WHERE team_long_name = 'Bologna';

-- Select the season, date, home_goal, and away_goal columns
SELECT 
	season,
	date,
	home_goal,
	away_goal
FROM matches_italy
WHERE
-- Exclude games not won by Bologna
	CASE WHEN hometeam_id = 9857 AND home_goal > away_goal THEN 'Bologna Win'
         WHEN awayteam_id = 9857 AND away_goal > home_goal THEN 'Bologna Win' 
         END IS NOT NULL;
```
