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


## Summary CASE WHEN with aggregate functions | SQL

This tutorial explains how to use CASE statements with aggregate functions in SQL to categorize data, filter information, count occurrences, calculate sums and averages, and compute percentages efficiently.

### Facts
- CASE statements categorize and filter data, including aggregation in WHERE clauses.
- Using COUNT with CASE allows counting based on logical tests, e.g., counting wins for specific conditions.
- CASE statements can be included inside aggregate functions like COUNT, SUM, and AVG to process data.
- SUM function calculates totals based on conditions, e.g., total goals scored by a team.
- AVG function computes averages of data, like average goals per season, and can be rounded for readability.
- Using CASE with AVG helps compute percentages, like the percentage of games won by a team in a season.

```
--Create a CASE statement that identifies the id of matches played in the 2012/2013 season. Specify that you want ELSE values to be NULL.
SELECT 
	c.name AS country,
    -- Count games from the 2012/2013 season
	COUNT(CASE WHEN m.season = '2012/2013' 
          	   THEN m.id ELSE NULL END) AS matches_2012_2013
FROM country AS c
LEFT JOIN match AS m
ON c.id = m.country_id
-- Group by country name alias
GROUP BY country;

--Create 3 CASE WHEN statements counting the matches played in each country across the 3 seasons.
SELECT 
	c.name AS country,
    -- Count matches in each of the 3 seasons
	COUNT(CASE WHEN m.season = '2012/2013' THEN m.id END) AS matches_2012_2013,
	COUNT(CASE WHEN m.season = '2013/2014' THEN m.id END) AS matches_2013_2014,
	COUNT(CASE WHEN m.season = '2014/2015' THEN m.id END) AS matches_2014_2015
FROM country AS c
LEFT JOIN match AS m
ON c.id = m.country_id
-- Group by country name alias
GROUP BY country;

--Create 3 CASE statements to "count" matches in the '2012/2013', '2013/2014', and '2014/2015' seasons, respectively.Have each CASE statement return a 1 for every match you want to include, and a 0 for every match to exclude.Wrap the CASE statement in a SUM to return the total matches played in each season.Group the query by the country name alias.
SELECT 
	c.name AS country,
    -- Sum the total records in each season where the home team won
	SUM(CASE WHEN m.season = '2012/2013' AND m.home_goal > m.away_goal 
        THEN 1 ELSE 0 END) AS matches_2012_2013,
	SUM(CASE WHEN m.season = '2013/2014' AND m.home_goal > m.away_goal 
        THEN 1 ELSE 0 END) AS matches_2013_2014,
	SUM(CASE WHEN m.season = '2014/2015' AND m.home_goal > m.away_goal 
        THEN 1 ELSE 0 END) AS matches_2014_2015
FROM country AS c
LEFT JOIN match AS m
ON c.id = m.country_id
-- Group by country name alias
GROUP BY country;


SELECT 
	c.name AS country,
    -- Round the percentage of tied games to 2 decimal points
	ROUND(AVG(CASE WHEN m.season='2013/2014' AND m.home_goal = m.away_goal THEN 1
			 WHEN m.season='2013/2014' AND m.home_goal != m.away_goal THEN 0
			 END),2) AS pct_ties_2013_2014,
	ROUND(AVG(CASE WHEN m.season='2014/2015' AND m.home_goal = m.away_goal THEN 1
			 WHEN m.season='2014/2015' AND m.home_goal != m.away_goal THEN 0
			 END),2) AS pct_ties_2014_2015
FROM country AS c
LEFT JOIN matches AS m
ON c.id = m.country_id
GROUP BY country;
```
Summary WHERE are the Subqueries? | SQL
Subqueries, nested queries within SQL statements, aid in data extraction, transformation, and filtering. They're placed within various SQL clauses and assist in comparing summarized values to detailed data.

Facts
- Subqueries are queries nested within other queries, aiding in data transformations before selecting, filtering, or calculating information.
- They can be placed in SELECT, FROM, WHERE, or GROUP BY clauses to return scalar quantities, lists for filtering or joining, or tables for further data manipulation.
- They're essential for comparing summarized data to detailed information, restructuring data for multiple purposes, and combining data from unjoinable tables.
- Simple subqueries, evaluated once in the entire query, offer standalone query capabilities and streamline data processing.
- Subqueries in the WHERE clause allow filtering results based on calculated values, reducing manual steps in querying specific data sets.
- Utilizing subqueries for generating filtering lists is valuable, aiding in comparisons between tables lacking direct connections.

 ```
--Calculate triple the average home + away goals scored across all matches. This will become your subquery in the next step. Note that this column does not have an alias, so it will be called ?column? in your results.
SELECT 
-- Select the average of home + away goals, multiplied by 3
	3 * AVG(home_goal + away_goal)
FROM matches_2013_2014;

--Create a subquery in the WHERE clause that retrieves all unique hometeam_ID values from the match table. Select the team_long_name and team_short_name from the team table. Exclude all values from the subquery in the main query.

SELECT 
	-- Select the team long and short names
	team_long_name,
	team_short_name
FROM team 
-- Exclude all values from the subquery
WHERE team_api_id not in
     (select DISTINCT hometeam_id  FROM match);

--Create a subquery in WHERE clause that retrieves all hometeam_ID values from match with a home_goal score greater than or equal to 8.Select the team_long_name and team_short_name from the team table. Include all values from the subquery in the main query.

SELECT
	-- Select the team long and short names
	team_long_name,
	team_short_name
FROM team
-- Filter for teams with 8 or more home goals
WHERE team_api_id IN
	  (SELECT hometeam_id 
       FROM match
       WHERE home_goal >= 8);

``` 

## Summary Subqueries in FROM | SQL
Subqueries in the FROM statement enable restructuring and transforming data for complex analysis. They're useful for preparing data, performing calculations, and obtaining specific results by nesting queries within the FROM clause.

### Facts
- Subqueries in the FROM statement allow restructuring data and preparing it for analysis.
- Useful for transforming data into different shapes or pre-filtering before calculations.
- Helpful in calculating aggregates of aggregate information, like finding top performers based on derived calculations.
- Creating a subquery involves selecting specific columns, applying functions (e.g., AVG), and filtering data.
- Subqueries can be incorporated into main queries by placing them in the FROM clause, giving them aliases, and joining them as necessary.
-Remember to assign aliases to subqueries, join them properly, and utilize columns for JOIN operations.
### Additional Tips
- Multiple subqueries can be created in the FROM statement; ensure proper aliasing and join conditions.
- Subqueries can be joined with existing tables; validate columns for join conditions.

```
--Create the subquery to be used in the next step, which selects the country ID and match ID (id) from the match table.Filter the query for matches with greater than or equal to 10 goals.
SELECT 
	-- Select the country ID and match ID
	country_id, 
    id 
FROM match
-- Filter for matches with 10 or more goals in total
WHERE (home_goal + away_goal) >= 10;

--Construct a subquery that selects only matches with 10 or more total goals.Inner join the subquery onto country in the main query & Select name from country and count the id column from match.
SELECT
	-- Select country name and the count match IDs
    name AS country_name,
    COUNT(country_id) AS matches
FROM country AS c
-- Inner join the subquery onto country
-- Select the country id and match id columns
inner join (SELECT country_id, id 
           FROM match
           -- Filter the subquery by matches with 10+ goals
           WHERE (home_goal + away_goal) >=10) AS sub
ON c.id = sub.country_id
GROUP BY country_name;

--Complete the subquery inside the FROM clause. Select the country name from the country table, along with the date, the home goal, the away goal, and the total goals columns from the match table.
SELECT
	-- Select country, date, home, and away goals from the subquery
    country,
    date,
    home_goal,
    away_goal
FROM
	-- Select country name, date, home_goal, away_goal, and total goals in the subquery
	(SELECT c.name AS country, 
     	    m.date, 
     		m.home_goal, 
     		m.away_goal,
           (m.home_goal + m.away_goal) AS total_goals
    FROM match AS m
    LEFT JOIN country AS c
    ON m.country_id = c.id) AS subquery
-- Filter by total goals scored in the main query
WHERE total_goals >= 10;

```
## Summary Subqueries in SELECT | SQL
Subqueries in SELECT statements allow for aggregating data and performing calculations within SQL queries, aiding in obtaining summary values within detailed datasets.

### Facts
- Subqueries in SELECT retrieve single aggregate values, facilitating the inclusion of aggregate values in ungrouped SQL queries.
- Subqueries enable complex mathematical calculations within datasets, like measuring deviations from averages or comparing values.
- Adding a subquery directly into the SELECT statement can yield identical results as including a separate calculated value.
- These subqueries are valuable for performing calculations within SQL queries, such as determining differences from averages or computing values based on existing data.
- Subqueries in SELECT must return a single value to avoid errors; multiple rows from a subquery will cause the query to fail. Also, proper filtering in both the main and subqueries is crucial for accurate results.
  
```
/*In the subquery, select the average total goals by adding home_goal and away_goal.
Filter the results so that only the average of goals in the 2013/2014 season is calculated.
In the main query, select the average total goals by adding home_goal and away_goal. This calculates the average goals for each league.
Filter the results in the main query the same way you filtered the subquery. Group the query by the league name.
*/
SELECT 
	l.name AS league,
    -- Select and round the league's total goals
    ROUND(AVG(m.home_goal + m.away_goal),2) AS avg_goals,
    -- Select and round the average total goals
    (SELECT ROUND(AVG(home_goal + away_goal),2) 
     FROM match
     WHERE season = '2013/2014') AS overall_avg
FROM league AS l
LEFT JOIN match AS m
ON l.country_id = m.country_id
-- Filter for the 2013/2014 season
WHERE m.season = '2013/2014'
GROUP BY l.name;


```
