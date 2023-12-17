
## Summary Fetching | SQL
This tutorial delves into practical applications of window functions, covering fetching values from different parts of a table into one row.

### Facts
- Relative Functions: LAG and LEAD retrieve values relative to the current row. LAG gets values preceding the current row, while LEAD retrieves values after the current row. FIRST_VALUE and LAST_VALUE are absolute functions that fetch the first and last values in a table/partition, respectively.
- LEAD: Demonstrates fetching the next city/cities after each set of Olympic Games. LEAD returns null when there are no subsequent rows to fetch, handling scenarios like the end of data.
- FIRST_VALUE and LAST_VALUE: Fetch the first and last cities of Olympic Games, showcasing absolute functions. LAST_VALUE's usage with the RANGE BETWEEN clause extends the window to capture the actual last value beyond the current row.
-  Partitioning with LEAD: Illustrates the impact of partitioning on fetching current and next champions, emphasizing the difference between partitioned and unpartitioned tables.
-  Partitioning with FIRST_VALUE: Expands on partitioning's effect, correctly assigning values based on partitions, highlighting the differences between partitioned and unpartitioned tables.


```
--For each year, fetch the current gold medalist and the gold medalist 3 competitions ahead of the current row.
WITH Discus_Medalists AS (
  SELECT DISTINCT
    Year,
    Athlete
  FROM Summer_Medals
  WHERE Medal = 'Gold'
    AND Event = 'Discus Throw'
    AND Gender = 'Women'
    AND Year >= 2000)

SELECT
  -- For each year, fetch the current and future medalists
  year,
  Athlete,
  lead(Athlete,3) OVER (ORDER BY year ASC) AS Future_Champion
FROM Discus_Medalists
ORDER BY Year ASC;

--Return all athletes and the first athlete ordered by alphabetical order.
WITH All_Male_Medalists AS (
  SELECT DISTINCT
    Athlete
  FROM Summer_Medals
  WHERE Medal = 'Gold'
    AND Gender = 'Men')

SELECT
  -- Fetch all athletes and the first athlete alphabetically
  athlete,
  first_value (athlete) OVER (
    ORDER BY athlete ASC
  ) AS First_Athlete
FROM All_Male_Medalists;
/*
Return the year and the city in which each Olympic games were held.
Fetch the last city in which the Olympic games were held.
*/
WITH Hosts AS (
  SELECT DISTINCT Year, City
    FROM Summer_Medals)

SELECT
  Year,
  City,
  -- Get the last city in which the Olympic games were held
  LAST_VALUE(City) OVER (
   ORDER BY Year ASC
   RANGE BETWEEN
     UNBOUNDED PRECEDING AND
     UNBOUNDED FOLLOWING
  ) AS Last_City
FROM Hosts
ORDER BY Year ASC;

```

## Summary  Window Functions and Ranking:
- ROW_NUMBER: Assigns unique numbers to rows, even if values are the same.
- RANK: Assigns the same number to identical values, skips over next numbers in such cases.
- DENSE_RANK: Assigns the same number to identical values, but doesn't skip over the next numbers.

## Different Functions in Action:
- ROW_NUMBER: Assigns unique numbers based on order, handles duplicates uniquely.
- RANK: Assigns the same number to equal values, skips next ranks.
- DENSE_RANK: Assigns the same number to equal values, doesn't skip ranks.
- Last rank differs: ROW_NUMBER and RANK have the count of rows, while DENSE_RANK has the count of unique values ranked.

## Ranking without Partitioning vs. with Partitioning:
- Without partitioning: Ranking without considering separate groups results in incorrect relative rankings.
- With partitioning: Correct ranking within specific groups achieved by partitioning.

```
--Rank each athlete by the number of medals they've earned -- the higher the count, the higher the rank -- with identical numbers in case of identical values.
WITH Athlete_Medals AS (
  SELECT
    Athlete,
    COUNT(*) AS Medals
  FROM Summer_Medals
  GROUP BY Athlete)

SELECT
  Athlete,
  Medals,
  -- Rank athletes by the medals they've won
  rank() OVER (ORDER BY medals DESC) AS Rank_N
FROM Athlete_Medals
ORDER BY Medals DESC;

--Rank each country's athletes by the count of medals they've earned -- the higher the count, the higher the rank -- without skipping numbers in case of identical values.
WITH Athlete_Medals AS (
  SELECT
    Country, Athlete, COUNT(*) AS Medals
  FROM Summer_Medals
  WHERE
    Country IN ('JPN', 'KOR')
    AND Year >= 2000
  GROUP BY Country, Athlete
  HAVING COUNT(*) > 1)

SELECT
  Country,
  -- Rank athletes in each country by the medals they've won
  Athlete,
  DENSE_RANK() OVER (PARTITION BY Country
                         ORDER BY Medals DESC) AS Rank_N
FROM Athlete_Medals
ORDER BY Country ASC, RANK_N ASC;

```
## Window functions' third common application: Paging
### Paging: Splitting data into approximately equal chunks
- APIs use "pages" to reduce data size exchange between platforms
- Sorting data into quartiles or thirds for performance assessment
### Paging in SQL
- **NTILE**: Window function splitting data into n equal pages
- NTILE example with 67 disciplines divided into 15 pages
- Uneven distribution due to 67 not divisible by 15, causing overflow in some pages
- **Top, middle, and bottom thirds**:
- NTILE divides data into thirds or quartiles
- Example: Country_Medals CTE categorizing countries by medal count into thirds
- Labels top, middle, or bottom x% of data based on medals awarded
- **Thirds averages**:
- Grouping by Third column shows average medal count per third
- Top third: average count of almost 600 medals, middle third: around 23, bottom third: approximately 2 medals
- **Practice exercises** with NTILE for paging and labeling data's top, middle, and bottom percentages
```
--Split the distinct events into exactly 111 groups, ordered by event in alphabetical order.
WITH Events AS (
  SELECT DISTINCT Event
  FROM Summer_Medals)
  
SELECT
  --- Split up the distinct events into 111 unique groups
  Event,
  ntile(111) OVER (ORDER BY Event ASC) AS Page
FROM Events
ORDER BY Event ASC;

--Split the athletes into top, middle, and bottom thirds based on their count of medals.
WITH Athlete_Medals AS (
  SELECT Athlete, COUNT(*) AS Medals
  FROM Summer_Medals
  GROUP BY Athlete
  HAVING COUNT(*) > 1)
  
SELECT
  Athlete,
  Medals,
  -- Split athletes into thirds by their earned medals
  ntile(3) over(order by medals desc)  AS Third
FROM Athlete_Medals
ORDER BY Medals DESC, Athlete ASC;

--Return the average of each third.

WITH Athlete_Medals AS (
  SELECT Athlete, COUNT(*) AS Medals
  FROM Summer_Medals
  GROUP BY Athlete
  HAVING COUNT(*) > 1),
  
  Thirds AS (
  SELECT
    Athlete,
    Medals,
    NTILE(3) OVER (ORDER BY Medals DESC) AS Third
  FROM Athlete_Medals)
  
SELECT
  -- Get the average medals earned in each third
  Third,
  AVG(Medals) AS Avg_Medals
FROM Thirds
GROUP BY Third
ORDER BY Third ASC;



```
