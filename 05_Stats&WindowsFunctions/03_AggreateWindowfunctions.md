## Chapter 3 builds on knowledge from Chapter 2, focusing on aggregate functions in a new context.
- Setting up a source table to count gold medals earned by Brazil across five sets of Olympic games.
- Using MAX and SUM aggregate functions on the Medals column within a CTE.
- MAX Window function: Utilizing MAX, SUM, COUNT, MIN, and AVG as window functions, showing max medals earned so far for each row based on ascending order of years.
- SUM Window function: Demonstrating cumulative sum or running total of medals earned so far using the same window defined for MAX.
- Partitioning with aggregate window functions: Explaining how to partition with aggregate functions, showcasing cumulative sum without partitioning and with partitioning by country.
- Practice exercises provided to apply knowledge of aggregate functions in a new context.
  
```
--Return the athletes, the number of medals they earned, and the medals running total, ordered by the athletes' names in alphabetical order.
WITH Athlete_Medals AS (
  SELECT
    Athlete, COUNT(*) AS Medals
  FROM Summer_Medals
  WHERE
    Country = 'USA' AND Medal = 'Gold'
    AND Year >= 2000
  GROUP BY Athlete)

SELECT
  -- Calculate the running total of athlete medals
  Athlete,
  Medals,
  SUM(Medals) OVER (ORDER BY Athlete ASC) AS Max_Medals
FROM Athlete_Medals
ORDER BY Athlete ASC;

--Return the year, country, medals, and the maximum medals earned so far for each country, ordered by year in ascending order.
WITH Country_Medals AS (
  SELECT
    Year, Country, COUNT(*) AS Medals
  FROM Summer_Medals
  WHERE
    Country IN ('CHN', 'KOR', 'JPN')
    AND Medal = 'Gold' AND Year >= 2000
  GROUP BY Year, Country)

SELECT
  -- Return the max medals earned so far per country
  Country,
  Year,
  Medals,
  MAX(Medals) OVER (PARTITION BY Country
                        ORDER BY Year ASC) AS Max_Medals
FROM Country_Medals
ORDER BY Country ASC, Year ASC;

--Return the year, medals earned, and minimum medals earned so far.
WITH France_Medals AS (
  SELECT
    Year, COUNT(*) AS Medals
  FROM Summer_Medals
  WHERE
    Country = 'FRA'
    AND Medal = 'Gold' AND Year >= 2000
  GROUP BY Year)

SELECT
  Year,
  Medals,
  MIN(Medals) OVER (ORDER BY Year ASC) AS Min_Medals
FROM France_Medals
ORDER BY Year ASC;
```

## Summary Frames | SQL

### Frames and Window Functions
- Use PARTITION and ORDER to alter window function behavior.
- Frames redefine how window functions operate.

### Motivation for Frames

- Frames avoid default behaviors like identical column values.
- Demonstrated using LAST_VALUE and frame clauses.

### Defining Frames: ROWS BETWEEN
- Frames start with RANGE BETWEEN or ROWS BETWEEN.
- ROWS BETWEEN: PRECEDING, CURRENT ROW, FOLLOWING define frame boundaries.
```
-- Return the year, medals earned, and the maximum medals earned, comparing only the current year and the next year.
WITH Scandinavian_Medals AS (
  SELECT
    Year, COUNT(*) AS Medals
  FROM Summer_Medals
  WHERE
    Country IN ('DEN', 'NOR', 'FIN', 'SWE', 'ISL')
    AND Medal = 'Gold'
  GROUP BY Year)

SELECT
  -- Select each year's medals
  Year,
  Medals,
  -- Get the max of the current and next years'  medals
  MAX(Medals) OVER (ORDER BY Year ASC
                    ROWS BETWEEN CURRENT ROW
                    AND 1 FOLLOWING) AS Max_Medals
FROM Scandinavian_Medals
ORDER BY Year ASC;

--Return the athletes, medals earned, and the maximum medals earned, comparing only the last two and current athletes, ordering by athletes' names in alphabetical order.
WITH Chinese_Medals AS (
  SELECT
    Athlete, COUNT(*) AS Medals
  FROM Summer_Medals
  WHERE
    Country = 'CHN' AND Medal = 'Gold'
    AND Year >= 2000
  GROUP BY Athlete)

SELECT
  -- Select the athletes and the medals they've earned
  Athlete,
  Medals,
  -- Get the max of the last two and current rows' medals 
  MAX(Medals) OVER (ORDER BY Athlete ASC
                    ROWS BETWEEN 2 PRECEDING
                    AND CURRENT ROW) AS Max_Medals
FROM Chinese_Medals
ORDER BY Athlete ASC;

```
