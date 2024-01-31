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
```
