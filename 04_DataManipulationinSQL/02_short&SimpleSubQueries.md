
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
## Summary Subqueries everywhere! And best practices! | SQL
 Subqueries are extensively used in SQL queries across various clauses, and understanding best practices like formatting, annotating, and filtering is crucial for readability and efficiency.

### Facts
- SQL allows multiple subqueries in different clauses within a query, but readability can suffer with longer, complex queries.
- Properly formatting queries (lining up statements) aids readability and comprehension for yourself and collaborators.
- Annotating queries with comments (using multiple-line or in-line comments) clarifies their purpose and functions.
- Indenting queries, especially within subqueries, enhances readability, aids in tracking changes, and managing complex statements.
- Clear indentation within single columns (e.g., long CASE statements) assists in understanding the conditions and outcomes.
- Assessing the necessity of subqueries is vital due to the added computational load they bring to query execution.
- Properly filtering each subquery and the main query ensures accurate and relevant results are generated.
```
/*
Extract the average number of home and away team goals in two SELECT subqueries.
Calculate the average home and away goals for the specific stage in the main query.
Filter both subqueries and the main query so that only data from the 2012/2013 season is included.
Group the query by the m.stage column.
*/

SELECT 
	-- Select the stage and average goals for each stage
	m.stage,
	ROUND(AVG(m.home_goal + m.away_goal),2) AS avg_goals,
    -- Select the average overall goals for the 2012/2013 season
	ROUND((SELECT AVG(home_goal + away_goal) 
           FROM match 
           WHERE season = '2012/2013'),2) AS overall
FROM match AS m
-- Filter for the 2012/2013 season
WHERE m.season = '2012/2013'
-- Group by stage
GROUP BY m.stage;

/*
Calculate the average home goals and average away goals from the match table for each stage in the FROM clause subquery.
Add a subquery to the WHERE clause that calculates the overall average home goals.
Filter the main query for stages where the average home goals is higher than the overall average.
Select the stage and avg_goals columns from the s subquery into the main query.
*/
SELECT 
	-- Select the stage and average goals from the subquery
	s.stage,
    ROUND(s.avg_goals,2) AS avg_goals
FROM 
	-- Select the stage and average goals in 2012/2013
	(SELECT
         stage,
         AVG(home_goal + away_goal) AS avg_goals
     FROM match
     WHERE season = '2012/2013'
     GROUP BY stage) AS s
WHERE 
	-- Filter the main query using the subquery
	s.avg_goals > (SELECT AVG(home_goal + away_goal) 
                   FROM match WHERE season = '2012/2013');
/*
Create a subquery in SELECT that yields the average goals scored in the 2012/2013 season. Name the new column overall_avg.
Create a subquery in FROM that calculates the average goals scored in each stage during the 2012/2013 season.
Filter the main query for stages where the average goals exceeds the overall average in 2012/2013.
*/
SELECT 
	-- Select the stage and average goals from s
	s.stage,
	ROUND(s.avg_goals,2) AS avg_goal,
    -- Select the overall average for 2012/2013
	(SELECT AVG(home_goal + away_goal) FROM match WHERE season = '2012/2013') AS overall_avg
FROM 
	-- Select the stage and average goals in 2012/2013 from match
	(SELECT
         stage,
         AVG(home_goal + away_goal) AS avg_goals
     FROM match
     WHERE season = '2012/2013'
     GROUP BY stage) AS s
WHERE 
	-- Filter the main query using the subquery
	s.avg_goals > (SELECT AVG(home_goal + away_goal) 
                   FROM match WHERE season = '2012/2013');
```

