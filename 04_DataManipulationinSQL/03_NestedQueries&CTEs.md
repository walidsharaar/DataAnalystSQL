
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
