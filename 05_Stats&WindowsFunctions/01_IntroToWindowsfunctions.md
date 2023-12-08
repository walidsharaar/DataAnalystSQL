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
```

