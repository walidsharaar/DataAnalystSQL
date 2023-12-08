## Summary Windows Functions | SQL
Window functions in SQL enable calculations on result sets without needing to group data, allowing operations like aggregations and ranking without aggregating entire datasets.

## Facts
- Subquery Transformation: Experience gained in using simple subqueries, correlated subqueries, and common table expressions to transform data.
- Limitation in Aggregates: SQL's limitation requiring grouping when using aggregate functions without matching non-aggregate values.
- Introduction to Window Functions: Window functions perform calculations on result sets ("windows") without requiring data grouping, allowing aggregate calculations and various insights like running totals, rankings, and moving averages.
- Understanding Window Functions: Window functions are executed via the OVER clause, enabling operations like comparing aggregate values to non-aggregate data without using subqueries.
- Generating Ranks: Window functions, such as RANK, create ordered data sets based on specified columns, offering insights into rankings based on certain criteria.
- Ordering in Ranks: Adjustment of ranks is possible by using the ORDER BY clause within the OVER clause, enabling ascending or descending ranking.
- Key Considerations: Window functions operate on the result set post-query processing (except final ordering), available in PostgreSQL, Oracle, MySQL, but not in SQLite.
 ```
 -- Select the match ID, country name, season, home, and away goals from the match and country tables.Complete the query that calculates the average number of goals scored overall and then includes the aggregate value in each row using a window function.
SELECT 
	-- Select the id, country name, season, home, and away goals
	m.id,
	c.name AS country,
	m.season,
	m.home_goal,
	m.away_goal,
    -- Use a window to include the aggregate average in each row
	AVG(m.home_goal + m.away_goal) OVER() AS overall_avg
FROM match AS m
LEFT JOIN country AS c ON m.country_id = c.id;
/*
Select the league name and average total goals scored from league and match.
Complete the window function so it calculates the rank of average goals scored across all leagues in the database.
Order the rank by the average total of home and away goals scored.
*/
SELECT 
	-- Select the league name and average goals scored
	l.name AS league,
    AVG(m.home_goal + m.away_goal) AS avg_goals,
    -- Rank each league according to the average goals
    RANK() OVER(ORDER BY AVG(m.home_goal + m.away_goal)) AS league_rank
FROM league AS l
LEFT JOIN match AS m 
ON l.id = m.country_id
WHERE m.season = '2011/2012'
GROUP BY l.name
-- Order the query by the rank you created
ORDER BY league_rank;

/*
Complete the same parts of the query as the previous exercise.
Complete the window function to rank each league from highest to lowest average goals scored.
Order the main query by the rank you just created.
*/
SELECT 
	-- Select the league name and average goals scored
	l.name AS league,
    AVG(m.home_goal + m.away_goal) AS avg_goals,
    -- Rank leagues in descending order by average goals
    RANK() OVER(ORDER BY AVG(m.home_goal + m.away_goal) DESC) AS league_rank
FROM league AS l
LEFT JOIN match AS m 
ON l.id = m.country_id
WHERE m.season = '2011/2012'
GROUP BY l.name
-- Order the query by the rank you created
ORDER BY league_rank;
 ```
## Summary OVER with a PARTITION | SQL
Window functions with the OVER clause allow for detailed computations within partitions, offering insights into data categorically.

### Facts
- OVER and PARTITION BY: The PARTITION BY statement within the OVER clause enables separate calculations for distinct categories in data, avoiding the need for separate columns for aggregate values.
- Partitioned Query Results: Queries utilizing PARTITION BY return data with calculated values specific to designated categories, like comparing match goals to overall and seasonal averages.
- Multiple Column Partitioning: PARTITION BY extends to multiple columns, breaking down average calculations by various categorical combinations, such as season and country.
- Versatile Usage: PARTITION BY works seamlessly with diverse window functions, accommodating different analysis needs efficiently.
 ```
-- Complete the two window functions that calculate the home and away goal averages. Partition the window functions by season to calculate separate averages for each season. Filter the query to only include matches played by Legia Warszawa, id = 8673.
SELECT 
	date,
	season,
    home_goal,
    away_goal,
    CASE WHEN hometeam_id = 8673 THEN 'home' 
         ELSE 'away' END AS warsaw_location,
    -- Calculate the average goals scored partitioned by season
    AVG(home_goal) OVER(PARTITION BY season) AS season_homeavg,
    AVG(away_goal) OVER(PARTITION BY season) AS season_awayavg
FROM match
-- Filter the data set for Legia Warszawa matches only
WHERE 
	hometeam_id = 8673 
    OR awayteam_id = 8673
ORDER BY (home_goal + away_goal) DESC;
--Construct two window functions partitioning the average of home and away goals by season and month. Filter the dataset by Legia Warszawa's team ID (8673) so that the window calculation only includes matches involving them.

SELECT 
	date,
    season,
    home_goal,
    away_goal,
    CASE WHEN hometeam_id = 8673 THEN 'home' 
         ELSE 'away' END AS warsaw_location,
    -- Calculate average goals partitioned by season and month
    AVG(home_goal) OVER(PARTITION BY season, 
         	EXTRACT(MONTH FROM date)) AS season_mo_home,
    AVG(away_goal) OVER(PARTITION BY season, 
            EXTRACT(MONTH FROM date)) AS season_mo_away
FROM match
WHERE 
	hometeam_id = 8673 
    OR awayteam_id = 8673
ORDER BY (home_goal + away_goal) DESC;
/*
Construct two window functions partitioning the average of home and away goals by season and month.
Filter the dataset by Legia Warszawa's team ID (8673) so that the window calculation only includes matches involving them.
*/
SELECT 
	date,
    season,
    home_goal,
    away_goal,
    CASE WHEN hometeam_id = 8673 THEN 'home' 
         ELSE 'away' END AS warsaw_location,
    -- Calculate average goals partitioned by season and month
    AVG(home_goal) OVER(PARTITION BY season, 
         	EXTRACT(MONTH FROM date)) AS season_mo_home,
    AVG(away_goal) OVER(PARTITION BY season, 
            EXTRACT(MONTH FROM date)) AS season_mo_away
FROM match
WHERE 
	hometeam_id = 8673 
    OR awayteam_id = 8673
ORDER BY (home_goal + away_goal) DESC;

```

## Summary Sliding windows | SQL
Window functions in SQL, particularly sliding windows, enable dynamic calculations based on rows in a dataset, allowing for diverse aggregations and rank computations.

### Facts
- Window functions calculate changing information relative to rows in a dataset.
- Sliding windows perform calculations based on the current row, enabling various aggregations like running totals, sums, and averages.
- Sliding window functions in SQL use keywords within the OVER clause to specify the data for calculations, like ROWS BETWEEN, PRECEDING, FOLLOWING, UNBOUNDED PRECEDING, UNBOUNDED FOLLOWING, and CURRENT ROW.
- An example illustrates using a sliding window to calculate a season's total goals, creating a running total ordered by match date.
- Using PRECEDING, a sliding window with a limited frame can compute specific aggregations, like summing goals in current and previous matches.

```
/*
Assessing the running total of home goals scored by FC Utrecht.
Assessing the running average of home goals scored.
Ordering both the running average and running total by date.
*/
SELECT 
	date,
	home_goal,
	away_goal,
    -- Create a running total and running average of home goals
	SUM(home_goal) OVER(ORDER BY date 
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total,
    AVG(home_goal) OVER(ORDER BY date 
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_avg
FROM match
WHERE 
	hometeam_id = 9908 
    AND season = '2011/2012';

/*
Complete the window function by:
Assessing the running total of home goals scored by FC Utrecht.
Assessing the running average of home goals scored.
Ordering both the running average and running total by date, descending.
*/

SELECT 
	-- Select the date, home goal, and away goals
	date,
	home_goal,
	away_goal,
    -- Create a running total and running average of home goals
    SUM(home_goal) OVER(ORDER BY date DESC
        ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING) AS running_total,
    AVG(home_goal) OVER(ORDER BY date DESC
        ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING) AS running_avg
FROM match
WHERE 
	awayteam_id = 9908 
    AND season = '2011/2012';

```
## Summary
This lesson ties together various SQL techniques learned in the course to address real queries.

### Facts
- Covered methods for transforming, manipulating, and calculating data in SQL.
- Learned CASE statements for categorizing, aggregating, and computing information.
- Explored subqueries in SELECT, FROM, and WHERE clauses, including nested and correlated ones.
- Utilized common table expressions to organize extensive data sets.
- Used various window functions to analyze data effectively.
- Case study: Identifying teams that defeated Manchester United in the 2013/2014 season.
- Constructed queries combining techniques like CASE statements, common table expressions, and window functions.
- Encouraged exploration of the European Soccer Database for personalized queries and analysis.

```
/*
Create a CASE statement that identifies each match as a win, lose, or tie for Manchester United.
Fill out the logical operators for each WHEN clause in the CASE statement (equals, greater than, less than).
Join the tables on home team ID from match, and team_api_id from team.
Filter the query to only include games from the 2014/2015 season where Manchester United was the home team.
*/
SELECT 
	m.id, 
    t.team_long_name,
    -- Identify matches as home/away wins or ties
	CASE when m.home_goal >m.away_goal  then 'MU Win'
		when m.home_goal < m.away_goal  then 'MU Loss'
        else 'Tie' end AS outcome
FROM match AS m
-- Left join team on the home team ID and team API id
LEFT JOIN team AS t 
ON m.hometeam_id = t.team_api_id
WHERE 
	-- Filter for 2014/2015 and Manchester United as the home team
	season = '2014/2015'
	AND t.team_long_name = 'Manchester United';
/*
Complete the CASE statement syntax.
Fill out the logical operators identifying each match as a win, loss, or tie for Manchester United.
Join the table on awayteam_id, and team_api_id.
*/
SELECT 
    m.id, 
	t.team_long_name,
    -- Identify matches as home/away wins or ties
	CASE WHEN m.home_goal > m.away_goal THEN 'MU Loss'
		 WHEN m.home_goal < m.away_goal THEN 'MU Win' 
         ELSE 'Tie' END AS outcome
-- Join team table to the match table
FROM match AS m
LEFT JOIN team AS t 
ON m.awayteam_id = t.team_api_id
WHERE 
	-- Filter for 2014/2015 and Manchester United as the away team
	m.season = '2014/2015'
	AND t.team_long_name = 'Manchester United';

/*
Declare the home and away CTEs before your main query.
Join your CTEs to the match table using a LEFT JOIN.
Select the relevant data from the CTEs into the main query.
Select the date from match, team names from the CTEs, and home/ away goals from match in the main query.
*/

-- Set up the home team CTE
WITH home AS (
  SELECT m.id, t.team_long_name,
	  CASE WHEN m.home_goal > m.away_goal THEN 'MU Win'
		   WHEN m.home_goal < m.away_goal THEN 'MU Loss' 
  		   ELSE 'Tie' END AS outcome
  FROM match AS m
  LEFT JOIN team AS t ON m.hometeam_id = t.team_api_id),
-- Set up the away team CTE
away AS (
  SELECT m.id, t.team_long_name,
	  CASE WHEN m.home_goal > m.away_goal THEN 'MU Loss'
		   WHEN m.home_goal < m.away_goal THEN 'MU Win' 
  		   ELSE 'Tie' END AS outcome
  FROM match AS m
  LEFT JOIN team AS t ON m.awayteam_id = t.team_api_id)
-- Select team names, the date and goals
SELECT DISTINCT
    m.date,
    home.team_long_name AS home_team,
    away.team_long_name AS away_team,
    m.home_goal, m.away_goal
-- Join the CTEs onto the match table
FROM match AS m
LEFT JOIN home ON m.id = home.id
LEFT JOIN away ON m.id = away.id
WHERE m.season = '2014/2015'
      AND (home.team_long_name = 'Manchester United' 
           OR away.team_long_name = 'Manchester United');

/*
Set up the CTEs so that the home and away teams each have a name, ID, and score associated with them.
Select the date, home team name, away team name, home goal, and away goals scored in the main query.
Rank the matches and order by the difference in scores in descending order.
*/

-- Set up the home team CTE
WITH home AS (
  SELECT m.id, t.team_long_name,
	  CASE WHEN m.home_goal > m.away_goal THEN 'MU Win'
		   WHEN m.home_goal < m.away_goal THEN 'MU Loss' 
  		   ELSE 'Tie' END AS outcome
  FROM match AS m
  LEFT JOIN team AS t ON m.hometeam_id = t.team_api_id),
-- Set up the away team CTE
away AS (
  SELECT m.id, t.team_long_name,
	  CASE WHEN m.home_goal > m.away_goal THEN 'MU Loss'
		   WHEN m.home_goal < m.away_goal THEN 'MU Win' 
  		   ELSE 'Tie' END AS outcome
  FROM match AS m
  LEFT JOIN team AS t ON m.awayteam_id = t.team_api_id)
-- Select columns and and rank the matches by goal difference
SELECT DISTINCT
    m.date,
    home.team_long_name AS home_team,
    away.team_long_name AS away_team,
    m.home_goal, m.away_goal,
    RANK() OVER(ORDER BY ABS(home_goal - away_goal) DESC) as match_rank
-- Join the CTEs onto the match table
FROM match AS m
LEFT JOIN home ON m.id = home.id
LEFT JOIN AWAY ON m.id = away.id
WHERE m.season = '2014/2015'
	  AND ((home.team_long_name = 'Manchester United' AND home.outcome = 'MU Loss')
	  OR (away.team_long_name = 'Manchester United' AND away.outcome = 'MU Loss'));



```

![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/e31ed0ad-14d4-4977-996f-4d0d1d6e24f2)
*Statement of Accomplishment*
