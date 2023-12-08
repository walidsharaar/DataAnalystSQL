
## Summary Correlated subqueries | SQL
Correlated subqueries involve nested queries that reference values from the outer query to generate specific results, impacting data filtering, calculations, and evaluations within a database.

### Facts
- Correlated subqueries refresh for each row in the final dataset, using outer query values to compute individual results.
- They're essential for intricate calculations, advanced joins, and precise data filtering.
- They replace certain join operations, simplifying queries while maintaining identical outcomes.
- Use them judiciously; their iterative nature can slow down query performance.


 ```
/*
Select the country_id, date, home_goal, and away_goal columns in the main query.
Complete the AVG value in the subquery.
Complete the subquery column references, so that country_id is matched in the main and subquery
*/

SELECT 
	-- Select country ID, date, home, and away goals from match
	main.country_id,
    main.date,
    main.home_goal,
    main.away_goal
FROM match AS main
WHERE 
	-- Filter the main query by the subquery
	(home_goal + away_goal) > 
        (SELECT AVG((sub.home_goal + sub.away_goal) * 3)
         FROM match AS sub
         -- Join the main query to the subquery in WHERE
         WHERE main.country_id = sub.country_id);

/*
Select the country_id, date, home_goal, and away_goal columns in the main query.
Complete the subquery: Select the matches with the highest number of total goals.
Match the subquery to the main query using country_id and season.
Fill in the correct logical operator so that total goals equals the max goals recorded in the subquery.
*/
SELECT 
	-- Select country ID, date, home, and away goals from match
	main.country_id,
    date,
    main.home_goal,
    main.away_goal
FROM match AS main
WHERE 
	-- Filter for matches with the highest number of goals scored
	(home_goal + away_goal)=
        (SELECT max(sub.home_goal + sub.away_goal)
         FROM match AS sub
         WHERE main.country_id = sub.country_id
               AND main.season = sub.season);

 ```
## Summary Nested subqueries | SQL

Nested subqueries involve embedding queries within queries, allowing for multi-layered data manipulation and analysis, providing comprehensive insights into specific datasets.

### Facts
- Nested subqueries refer to queries nested within other queries, aiding in data transformation and filtering.
- Data often needs multiple transformations before extraction, requiring layered subqueries for complex queries.
- Subqueries in SELECT statements enable comparisons between specific subsets and overall data.
- Nested subqueries can involve inner and outer subqueries to calculate averages and generate scalar values.
- Correlated nested subqueries allow for referencing outer or main query information, addressing specific problem-solving needs.
 
 ```
/*
Complete the main query to select the season and the max total goals in a match for each season. Name this max_goals.
Complete the first simple subquery to select the max total goals in a match across all seasons. Name this overall_max_goals.
Complete the nested subquery to select the maximum total goals in a match played in July across all seasons.
Select the maximum total goals in the outer subquery. Name this entire subquery july_max_goals.
*/
SELECT 
	-- Select the season and max goals scored in a match
	season,
    MAX(home_goal + away_goal) AS max_goals,
    -- Select the overall max goals scored in a match
   (SELECT MAX(home_goal + away_goal) FROM match) AS overall_max_goals,
    -- Select the max number of goals scored in any match in July
   (SELECT MAX(home_goal + away_goal) 
        FROM match
        WHERE id IN (
              SELECT id FROM match WHERE EXTRACT(MONTH FROM date) = 07)) AS july_max_goals
FROM match
GROUP BY season;

-- Count match ids
SELECT
    country_id,
    season,
    COUNT(id) AS matches
-- Set up and alias the subquery
FROM (
	SELECT
    	country_id,
    	season,
    	id
	FROM match
	WHERE home_goal >= 5 OR away_goal >= 5) 
    AS subquery
-- Group by country_id and season
GROUP BY country_id, season;
 ```
## Summary Common Table Expressions | SQL
Common table expressions (CTEs) are a method to enhance query readability and organization in SQL. They're declared ahead of the main query and behave like temporary tables, improving query management and performance.

### Facts
- CTEs, declared with the WITH statement, are used to name and reference subqueries separately before using them in the main query's FROM statement.
- They replace nested subqueries, making queries easier to read and manage by placing them at the query's beginning.
- CTEs offer performance benefits by running once and storing the results in memory, potentially improving query execution time.
- They allow referencing earlier CTEs, enabling complex query organization and providing a cleaner structure for multiple subqueries.
- Recursive CTEs enable a CTE to reference itself, opening avenues for more advanced query functionalities.
 ```
-- Set up your CTE
WITH match_list AS (
    SELECT 
  		country_id, 
  		id
    FROM match
    WHERE (home_goal + away_goal) >= 10)
-- Select league and count of matches from the CTE
SELECT
    l.name AS league,
    COUNT(match_list.id) AS matches
FROM league AS l
-- Join the CTE to the league table
LEFT JOIN match_list 
ON l.id = match_list.country_id
GROUP BY l.name;

/*
Declare your CTE, where you create a list of all matches with the league name.
Select the league, date, home, and away goals from the CTE.
Filter the main query for matches with 10 or more goals.
*/
-- Set up your CTE
WITH match_list AS (
  -- Select the league, date, home, and away goals
    SELECT 
  		l.name AS league, 
     	m.date, 
  		m.home_goal, 
  		m.away_goal,
       (m.home_goal + m.away_goal) AS total_goals
    FROM match AS m
    LEFT JOIN league as l ON m.country_id = l.id)
-- Select the league, date, home, and away goals from the CTE
SELECT league, date, home_goal, away_goal
FROM match_list
-- Filter by total goals
WHERE total_goals >= 10;

/*
Declare a CTE that calculates the total goals from matches in August of the 2013/2014 season.
Left join the CTE onto the league table using country_id from the match_list CTE.
Filter the list on the inner subquery to only select matches in August of the 2013/2014 season.
*/
-- Set up your CTE
WITH match_list AS (
    SELECT 
  		country_id, 
  	   (home_goal + away_goal) AS goals
    FROM match
    -- Create a list of match IDs to filter data in the CTE
    WHERE id IN (
       SELECT id
       FROM match
       WHERE season = '2013/2014' AND EXTRACT(MONTH FROM date) = 08))
-- Select the league name and average of goals in the CTE
SELECT
	l.name,
    AVG(match_list.goals)
FROM league AS l
-- Join the CTE onto the league table
LEFT JOIN match_list ON l.id = match_list.country_id
GROUP BY l.name;

 ```

## Summary Deciding on techniques to use | SQL

Choosing between various SQL techniques involves understanding their nuances and applications, allowing you to optimize queries for specific scenarios.

### Facts
- SQL techniques (joins, subqueries, CTEs) overlap but serve distinct purposes, affecting query efficiency and output accuracy.-
- Joins merge table data directly; correlated subqueries link a subquery with a table, circumventing join limitations.
- Multiple and nested subqueries handle multi-step transformations for precise query results.
- Common table expressions (CTEs) organize subqueries sequentially, aiding complex queries by referencing earlier data.
- Selection of technique depends on database, field, and query nature; practical experimentation aids in choosing the best approach.
- Each technique suits different use cases: joins for multiple table integration, correlated subqueries for matching data, nested subqueries for multi-step transformations, and CTEs for comparing diverse data pieces in a single query.

 ```
--Create a query that left joins team to match in order to get the identity of the home team. This becomes the subquery in the next step
SELECT 
	m.id, 
    t.team_long_name AS hometeam
-- Left join team to match
FROM match AS m
left join team as t
ON m.hometeam_id = team_api_id;

--Add a second subquery to the FROM statement to get the away team name, changing only the hometeam_id. Left join both subqueries to the match table on the id column.
SELECT
	m.date,
    -- Get the home and away team names
    hometeam,
    awayteam,
    m.home_goal,
    m.away_goal
FROM match AS m

-- Join the home subquery to the match table
LEFT JOIN (
  SELECT match.id, team.team_long_name AS hometeam
  FROM match
  LEFT JOIN team
  ON match.hometeam_id = team.team_api_id) AS home
ON home.id = m.id

-- Join the away subquery to the match table
LEFT JOIN (
  SELECT match.id, team.team_long_name AS awayteam
  FROM match
  LEFT JOIN team
  -- Get the away team ID in the subquery
  ON match.awayteam_id = team.team_api_id) AS away
ON away.id = m.id;

--Create a second correlated subquery in SELECT, yielding the away team's name.Select the home and away goal columns from match in the main query.
SELECT
    m.date,
    (SELECT team_long_name
     FROM team AS t
     WHERE t.team_api_id = m.hometeam_id) AS hometeam,
    -- Connect the team to the match table
    (SELECT team_long_name
     FROM team AS t
     WHERE team_api_id = awayteam_id) AS awayteam,
    -- Select home and away goals
     home_goal,
     away_goal
FROM match AS m;

--Let's declare the second CTE, away. Join it to the first CTE on the id column. The date, home_goal, and away_goal columns have been added to the CTEs. SELECT them into the main query.


WITH home AS (
  SELECT m.id, m.date, 
  		 t.team_long_name AS hometeam, m.home_goal
  FROM match AS m
  LEFT JOIN team AS t 
  ON m.hometeam_id = t.team_api_id),
-- Declare and set up the away CTE
away AS (
  SELECT m.id, m.date, 
  		 t.team_long_name AS awayteam, m.away_goal
  FROM match AS m
  LEFT JOIN team AS t 
  ON m.awayteam_id = t.team_api_id)
-- Select date, home_goal, and away_goal
SELECT 
	home.date,
    home.hometeam,
    away.awayteam,
    home.home_goal,
    away.away_goal
-- Join away and home on the id column
FROM home
INNER JOIN away
ON home.id = away.id;
 ```
