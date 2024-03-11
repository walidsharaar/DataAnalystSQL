## Summary Numeric data types and summary functions 

This chapter covers numeric data types, including integer and decimal types, and explores summary functions for analyzing numeric data.

### Facts
- Numeric data includes integer and decimal types with varying precision.
- Integer types include int, smallint, bigint, and serial, with different ranges.
- Serial types are used for autoincrementing integer columns, often for generating unique IDs.
- Decimal types (decimal and numeric) offer high precision, while real and double precision provide less precision.
- Division behaves differently for integers and decimals, affecting the result.
- Check the range of values in a column using min and max functions.
- Use avg function for the average or mean of column values.
- Explore variance with var_pop for population variance and var_samp for sample variance.
- Understand standard deviation, calculated as the square root of variance.
- Round numeric values to a specified decimal place using the round function.
- Summarize variables by groups, such as tags in a table, for a more detailed analysis.

```
/*
Compute revenue per employee by dividing revenues by employees; use casting to produce a numeric result.
Take the average of revenue per employee with avg(); alias this as avg_rev_employee.
Group by sector.
Order by the average revenue per employee.
*/
SELECT sector, 
       avg(revenues/employees::numeric) AS avg_rev_employee
  FROM fortune500
 GROUP BY sector
 ORDER BY avg_rev_employee;


-- Divide unanswered_count by question_count
  -- What are you comparing the above quantity to?
  -- Select rows where question_count is not 0

SELECT unanswered_count/question_count::numeric AS computed_pct, 
       unanswered_pct
  FROM stackoverflow
 WHERE question_count != 0
 LIMIT 10;

-- Select min, avg, max, and stddev of fortune500 profits
SELECT min(profits),
       avg(profits),
       max(profits),
       stddev(profits)
  FROM fortune500;

-- Select sector and summary measures of fortune500 profits
SELECT sector,
       min(profits),
       avg(profits),
       max(profits),
       stddev(profits)
  FROM fortune500
 -- What to group by?
 GROUP BY sector
 -- Order by the average profits
 ORDER BY avg;

-- Compute standard deviation of maximum values
SELECT stddev(maxval),
       -- min
       min(maxval),
       -- max
       max(maxval),
       -- avg
       avg(maxval)
  -- Subquery to compute max of question_count by tag
  FROM (SELECT max(question_count) AS maxval
          FROM stackoverflow
         -- Compute max by...
         GROUP BY tag) AS max_results; -- alias for subquery
```

## Summary Exploring distributions | SQL

Understanding variable distribution is crucial for identifying data errors, outliers, and anomalies. This process involves counting values, truncating, and grouping data to create meaningful bins.

### Facts
- Count values
       - Use grouping and ordering to analyze distributions for variables with a small number of discrete values.
       - Consider binning or grouping for variables with numerous values.
  Example: 20 distinct values in the unanswered_count column for the amazon-ebs tag.
- Truncate: Employ the trunc function to reduce numeric precision by replacing right-most digits with zeros. Positive and negative arguments determine digits after and before the decimal, respectively.
- Truncating and grouping: Group values in a column by the tens place using the trunc function with a negative second argument.
- Generate series: Utilize the generate_series function to group values by quantities other than place values. Example: Generate a series from 1 to 10 by steps of 2 or from 0 to 1 by steps of 1/10th.
- Create bins: Use generate_series to create bins with lower and upper values, along with the count of observations in each bin. Use a WITH clause to alias subquery results for later use. Generate series for lower and upper bounds, create a subset for amazon-ebs data, and write a query to join and count values. The output provides counts of days with unanswered questions falling within specified bin bounds, including bins with zero values.
ℹ️ Note: The approach counts non-null values of unanswered_count instead of just the number of rows.

```
-- Use trunc() to truncate employees to the 100,000s (5 zeros). Count the number of observations with each truncated value.
SELECT trunc(employees, -5) AS employee_bin,
       count(*)
  FROM fortune500
 GROUP BY employee_bin
 ORDER BY employee_bin;

--Repeat step 1 for companies with < 100,000 employees (most common). This time, truncate employees to the 10,000s place.
SELECT trunc(employees, -4) AS employee_bin,
       count(*)
  FROM fortune500
 WHERE employees < 100000
 GROUP BY employee_bin
 ORDER BY employee_bin;

-- Start by selecting the minimum and maximum of the question_count column for the tag 'dropbox' so you know the range of values to cover with the bins.
SELECT min(question_count), 
       max(question_count)
  FROM stackoverflow
 WHERE tag='dropbox';


-- Create lower and upper bounds of bins
SELECT generate_series(2200, 3050, 50) AS lower,
       generate_series(2250, 3100, 50) AS upper;

--Select lower and upper from bins, along with the count of values within each bin bounds.To do this, you'll need to join 'dropbox', which contains the question_count for tag "dropbox", to the bins created by generate_series().

-- Bins created in Step 2
WITH bins AS (
      SELECT generate_series(2200, 3050, 50) AS lower,
             generate_series(2250, 3100, 50) AS upper),
     dropbox AS (
      SELECT question_count 
        FROM stackoverflow
       WHERE tag='dropbox') 
SELECT lower, upper, count(question_count) 
  FROM bins
       LEFT JOIN dropbox
         ON question_count >= lower 
        AND question_count < upper
 GROUP BY lower, upper
 ORDER BY lower;
```

## More summary functions | SQL
This text introduces advanced functions for exploring numeric data, focusing on correlation, median, percentile functions, and common issues to consider in data analysis.

### Facts
- More Summary Functions: Introduction to additional functions for exploring numeric data.
- Correlation: Understanding the relationship between two variables using correlation coefficients.
- Correlation Function: corr function calculates the correlation between two columns, excluding rows with null values.
- Median: Describes the median as the 50th percentile or midpoint in a sorted list of values.
- Percentile Functions: Details on using percentile functions, including ordered-set aggregate syntax and differences between discrete and continuous percentile calculations.
= Percentile Examples: Explains how discrete and continuous percentile functions can yield different median values.
- Common Issues: Highlights potential issues with numeric data, such as error codes, special meanings, outlier values, and inappropriate numerical treatment of data types like zip codes.

```
--Compute the correlation between revenues and profits.
--Compute the correlation between revenues and assets.
--Compute the correlation between revenues and equity.

SELECT corr(revenues, profits) AS rev_profits,
       corr(revenues, assets) AS rev_assets,
       corr(revenues, equity) AS rev_equity 
  FROM fortune500;

--Compute the mean (avg()) and median assets of Fortune 500 companies by sector.Use the percentile_disc() function to compute the median.

SELECT sector, 
       avg(assets) AS mean,
       percentile_disc(0.5) WITHIN GROUP (ORDER BY assets) AS median
  FROM fortune500
 GROUP BY sector
 ORDER BY mean;
```
```
Compute the mean (avg()) and median assets of Fortune 500 companies by sector. Use the percentile_disc() function to compute the median: 1.percentile_disc(0.5) 2.WITHIN GROUP (ORDER BY column_name)

SELECT sector, 
       avg(assets) AS mean,
       percentile_disc(0.5) WITHIN GROUP (ORDER BY assets) AS median
  FROM fortune500
 GROUP BY sector
 ORDER BY mean;

```

## Summary Creating temporary tables | SQL

### Creating Temporary Tables
- Temporary tables are used to keep query results during a database session; they require no special permissions and are session-specific.
### Syntax
Temporary tables can be created with a "create temp table" command or a "select into" syntax, with Postgres recommending the former for additional options.
- Create a Table: Example shown for creating a temp table named top_companies with rank and title of the top 10 Fortune500 companies using "create temp table" syntax.
- Insert into Table: Rows can be added to an existing temp table using "insert into", demonstrated by adding ranks 11 to 20 companies.
- Delete (Drop) Table: Temp tables can be deleted with "drop table"; use "if exists" to avoid errors in scripts if the table doesn't exist.
### Time to Create Some Tables!

```
-- To clear table if it already exists;
DROP TABLE IF EXISTS profit80;

-- Create the temporary table
CREATE TEMP TABLE profit80 AS
  SELECT sector, 
         percentile_disc(0.8) WITHIN GROUP (ORDER BY profits) AS pct80
    FROM fortune500 
   GROUP BY sector;
   
-- See what you created: select all columns and rows 
-- from the table you created
SELECT * 
  FROM profit80;


--Using the profit80 table you created in step 1, select companies that have profits greater than pct80. Select the title, sector, profits from fortune500, as well as the ratio of the company's profits to the 80th percentile profit.

-- Code from previous step
DROP TABLE IF EXISTS profit80;

CREATE TEMP TABLE profit80 AS
  SELECT sector, 
         percentile_disc(0.8) WITHIN GROUP (ORDER BY profits) AS pct80
    FROM fortune500 
   GROUP BY sector;

SELECT title, fortune500.sector, 
       profits, profits/pct80 AS ratio
  FROM fortune500 
       LEFT JOIN profit80
       ON fortune500.sector=profit80.sector
 WHERE profits > pct80;

```

