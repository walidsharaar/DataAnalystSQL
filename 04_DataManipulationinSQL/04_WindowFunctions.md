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
![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/e31ed0ad-14d4-4977-996f-4d0d1d6e24f2)
*Statement of Accomplishment*
