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
