## Summary Pivoting | SQL
### Pivoting:
- Window functions offer unique insights beyond GROUP BY but maintain the result's structure.
- Transformation of ranking data from vertical to horizontal for easier scanning.

### Transforming tables:
- Pivoting alters tables by creating columns from unique values within a column.
- Pivoted tables enhance readability, especially when based on chronologically ordered columns.

### CROSSTAB:
- SQL's CROSSTAB facilitates table pivoting by a designated column.
- Use CREATE EXTENSION to access CROSSTAB within the tablefunc extension.
- Structure your source query within the dollar sign pairs and define the new pivoted table's columns.

```
-- Create the correct extension to enable CROSSTAB
CREATE EXTENSION IF NOT EXISTS tablefunc;

SELECT * FROM CROSSTAB($$
  SELECT
    Gender, Year, Country
  FROM Summer_Medals
  WHERE
    Year IN (2008, 2012)
    AND Medal = 'Gold'
    AND Event = 'Pole Vault'
  ORDER By Gender ASC, Year ASC
-- Fill in the correct column names for the pivoted table
$$) AS ct (Gender VARCHAR,
           "2008" VARCHAR,
           "2012" VARCHAR)

ORDER BY Gender ASC;

-- Count the gold medals per country and year
SELECT
  Country,
  Year,
  COUNT(*) AS Awards
FROM Summer_Medals
WHERE
  Country IN ('FRA', 'GBR', 'GER')
  AND Year IN (2004, 2008, 2012)
  AND Medal = 'Gold'
GROUP BY Country, Year
ORDER BY Country ASC, Year ASC

---Select the country and year columns, then rank the three countries by how many gold medals they earned per year.
WITH Country_Awards AS (
  SELECT
    Country,
    Year,
    COUNT(*) AS Awards
  FROM Summer_Medals
  WHERE
    Country IN ('FRA', 'GBR', 'GER')
    AND Year IN (2004, 2008, 2012)
    AND Medal = 'Gold'
  GROUP BY Country, Year)

SELECT
  Country,
  Year,
  -- Rank by gold medals earned per year
  RANK() OVER
    (PARTITION BY Year
     ORDER BY Awards DESC) :: INTEGER AS rank
FROM Country_Awards
ORDER BY Country ASC, Year ASC;


-- The unpivoted column is Country, and the pivoted column's unique values are 2004, 2008, and 2012. Make sure to surround the years with double quotes when listing them as column names.
CREATE EXTENSION IF NOT EXISTS tablefunc;

SELECT * FROM CROSSTAB($$
  WITH Country_Awards AS (
    SELECT
      Country,
      Year,
      COUNT(*) AS Awards
    FROM Summer_Medals
    WHERE
      Country IN ('FRA', 'GBR', 'GER')
      AND Year IN (2004, 2008, 2012)
      AND Medal = 'Gold'
    GROUP BY Country, Year)

  SELECT
    Country,
    Year,
    RANK() OVER
      (PARTITION BY Year
       ORDER BY Awards DESC) :: INTEGER AS rank
  FROM Country_Awards
  ORDER BY Country ASC, Year ASC;
-- Fill in the correct column names for the pivoted table
$$) AS ct (Country VARCHAR,
           "2004" INTEGER,
           "2008" INTEGER,
           "2012" INTEGER)

Order by Country ASC;

```

### Summary ROLLUP and CUBE | SQL
- ROLLUP and CUBE: Aggregate Functions in SQL
- Group-level Totals: Calculating Totals in SQL Queries
- The Old Way: Inelegant Methods for Totals Calculation
- Enter ROLLUP: Using ROLLUP for Group-Level Aggregations
- ROLLUP in Action: Generating Group and Grand Totals
- ROLLUP Results: Identifying Country and Grand Totals
- Enter CUBE: Utilizing CUBE for Aggregations
- CUBE Result: Identifying Country and Medal-Level Totals
- ROLLUP vs CUBE: Choosing the Right Subclause in SQL
```
--You want to look at three Scandinavian countries' earned gold medals per country and gender in the year 2004. You're also interested in Country-level subtotals to get the total medals earned for each country, but Gender-level subtotals don't make much sense in this case, so disregard them.

-- Count the gold medals per country and gender
SELECT
  Country,
  Gender,
  COUNT(*) AS Gold_Awards
FROM Summer_Medals
WHERE
  Year = 2004
  AND Medal = 'Gold'
  AND Country IN ('DEN', 'NOR', 'SWE')
-- Generate Country-level subtotals
GROUP BY Country, ROLLUP(Gender)
ORDER BY Country ASC, Gender ASC;

--  Count the medals awarded per gender and medal type. Generate all possible group-level counts (per gender and medal type subtotals and the grand total).
-- Count the medals per gender and medal type
SELECT
  Gender,
  Medal,
  COUNT(*) AS Awards
FROM Summer_Medals
WHERE
  Year = 2012
  AND Country = 'RUS'
-- Get all possible group-level subtotals
GROUP BY CUBE(Gender, Medal)
ORDER BY Gender ASC, Medal ASC;

```
