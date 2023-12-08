## Summary Introduction | SQL
This course, led by Michel Semaan and Fernando Gonzalez Prada, introduces window functions, simplifying complex SQL queries, and their application in analyzing the Summer Olympics dataset.

### Facts
- Motivation: Window functions eliminate the need for convoluted SQL queries by performing operations across related rows, allowing tasks like calculating running totals and fetching previous row values efficiently.
- Course Outline: Divided into four chapters, covering window function fundamentals, fetching/ranking values, using aggregate functions, and advanced techniques when combined with window functions.
- Summer Olympics Dataset: The dataset used throughout the course represents awarded medals, encompassing year, city, sport, athlete details, and medal types.
- Window Functions: They operate across rows while maintaining the entire row output, enabling tasks like fetching values from preceding or succeeding rows, assigning ranks, and calculating running totals/moving averages.
- Row Numbers: Fundamental in window functions, they assign indices to rows, allowing referencing based on position, independent of values.
- ROW_NUMBER: An example of a window function, assigns a sequential number to each row based on the specified window frame.
- Anatomy of a Window Function: The OVER clause in window functions can include subclauses like ORDER BY, PARTITION BY, and ROWS/FOLLOWING, impacting the function's output significantly. Understanding these subclauses is crucial for nuanced application.

This course provides a comprehensive understanding of window functions in SQL, offering efficient solutions for data analysis tasks within the context of the Summer Olympics dataset.
```
--Number each row in the dataset.
SELECT
  *,
  -- Assign numbers to each row
  ROW_NUMBER() OVER() AS Row_N
FROM Summer_Medals
ORDER BY Row_N ASC;

--Assign a number to each year in which Summer Olympic games were held.
SELECT
  Year,

  -- Assign numbers to each year
  row_number() over() AS Row_N
FROM (
  SELECT distinct year
  FROM Summer_Medals
  ORDER BY Year ASC
) AS Years
ORDER BY Year ASC;


```

## Summary ORDER BY | SQL

This tutorial dives into using the ORDER BY clause within the OVER clause in SQL window functions, illustrating its impact on row numbering, multi-column sorting, and applications like identifying reigning champions.

### Facts
- Basics of Window Function: Understanding the basics laid in the previous video, focusing on the OVER clause and its subclause, ORDER BY.
- Row Numbers Reversed: Exploring scenarios where row numbers are reversed, altering the sequence from ascending to descending.
- Effect of ORDER BY: Demonstrating how ORDER BY within OVER influences row numbering based on specified criteria like year, affecting the assigned numbers.
- Multiple Column Sorting: Exploring sorting by multiple columns within the OVER clause and its impact on row numbering.
- Ordering Inside & Outside OVER: Understanding the interplay of ordering inside and outside OVER clauses and its effect on result sequencing.
- Reigning Champions: Utilizing window functions like LAG to identify reigning champions by comparing current and previous year champions.
- Using LAG Function: Utilizing LAG function to retrieve previous row values without complex self-joins.
- Current Champions: Querying current year's champions as a step toward using LAG to access previous champions.
- Current and Last Champions: Employing Common Table Expressions (CTE) and LAG to display both current and previous champions in the results, observing how the last champion aligns with the previous year's outcome.

```
--Assign a number to each year in which Summer Olympic games were held so that rows with the most recent years have lower row numbers.
SELECT
  Year,
  -- Assign the lowest numbers to the most recent years
  ROW_NUMBER() OVER (ORDER BY Year DESC) AS Row_N
FROM (
  SELECT DISTINCT Year
  FROM Summer_Medals
) AS Years
ORDER BY Year desc;

--Select the Athlete column and count the number of rows each athlete appears in.
SELECT
  -- Count the number of medals each athlete has earned
  Athlete,
  COUNT(*) AS Medals
FROM Summer_Medals
GROUP BY Athlete
ORDER BY Medals DESC;

--Having wrapped the previous query in the Athlete_Medals CTE, rank each athlete by the number of medals they've earned.
WITH Athlete_Medals AS (
  SELECT
    -- Count the number of medals each athlete has earned
    Athlete,
    COUNT(*) AS Medals
  FROM Summer_Medals
  GROUP BY Athlete)

SELECT
  -- Number each athlete by how many medals they've earned
  athlete,
  row_number() OVER (ORDER BY medals DESC) AS Row_N
FROM Athlete_Medals
ORDER BY Medals DESC;

--Select the year and country columns, and filter out non-Gold medals.
SELECT
  -- Return each year's champions' countries
  Year,
  Country AS champion
FROM Summer_Medals
WHERE
  Discipline = 'Weightlifting' AND
  Event = '69KG' AND
  Gender = 'Men' AND
  Medal = 'Gold';

--Having wrapped the previous query in the Weightlifting_Gold CTE, get the previous year's champion for each year.
WITH Weightlifting_Gold AS (
  SELECT
    -- Return each year's champions' countries
    Year,
    Country AS champion
  FROM Summer_Medals
  WHERE
    Discipline = 'Weightlifting' AND
    Event = '69KG' AND
    Gender = 'Men' AND
    Medal = 'Gold')

SELECT
  Year, Champion,
  -- Fetch the previous year's champion
  LAG(Champion) OVER
    (ORDER BY Year ASC) AS Last_Champion
FROM Weightlifting_Gold
ORDER BY Year ASC;


```
## Summary PARTITION BY | SQL
Using the PARTITION BY subclause in SQL window functions divides data into partitions based on specified columns, allowing separate operations within each partition.

### Facts
- PARTITION BY in window functions changes behavior similarly to ORDER BY but divides data into partitions for distinct operations.
- It helps differentiate data sets within window functions, preventing cross-partition errors or misrepresentations.
- Unlike GROUP BY, PARTITION BY doesn't collapse results into a single column but operates on partitions individually.
- For instance, applying PARTITION BY Event in a query separates data into distinct partitions for each unique value in the Event column.
- Complex partitioning involves resetting row numbers across various criteria like country and year, achievable using multiple columns in PARTITION BY.
Understanding PARTITION BY in SQL window functions allows for precise data handling within distinct partitions, preventing errors and enabling granular analysis based on specified column values.
```
--Return the previous champions of each year's event by gender.
WITH Tennis_Gold AS (
  SELECT DISTINCT
    Gender, Year, Country
  FROM Summer_Medals
  WHERE
    Year >= 2000 AND
    Event = 'Javelin Throw' AND
    Medal = 'Gold')

SELECT
  Gender, Year,
  Country AS Champion,
  -- Fetch the previous year's champion by gender
  LAG(Country) OVER (PARTITION BY Gender
                         ORDER BY Year ASC) AS Last_Champion
FROM Tennis_Gold
ORDER BY Gender ASC, Year ASC;

--Return the previous champions of each year's events by gender and event.
WITH Athletics_Gold AS (
  SELECT DISTINCT
    Gender, Year, Event, Country
  FROM Summer_Medals
  WHERE
    Year >= 2000 AND
    Discipline = 'Athletics' AND
    Event IN ('100M', '10000M') AND
    Medal = 'Gold')

SELECT
  Gender, Year, Event,
  Country AS Champion,
  -- Fetch the previous year's champion by gender and event
  LAG(Country) OVER (PARTITION BY Gender, Event
                         ORDER BY Year ASC) AS Last_Champion
FROM Athletics_Gold
ORDER BY Event ASC, Gender ASC, Year ASC;


```

