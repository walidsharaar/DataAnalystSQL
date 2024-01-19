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

```
