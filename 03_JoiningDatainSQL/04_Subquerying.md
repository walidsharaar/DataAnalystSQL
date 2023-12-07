## Summary Subquerying with semi joins and anti joins | SQL
Subquerying and anti-joins introduce advanced SQL techniques beyond traditional JOIN operations. Semi joins filter data using the WHERE clause based on a condition, while anti joins select records where no match is found between columns.

### Facts
- The six familiar joins in SQL are "additive," adding columns to the original left table via INNER JOIN operations.
- INNER JOIN syntax adds fields with different names and duplicates fields with identical names, solvable through aliasing.
- Semi joins differ from traditional joins by leveraging the WHERE clause to filter records without using explicit JOIN keywords.
- A semi join selects values in the first table based on a condition met in the second table.
- Leveraging WHERE clauses and subqueries is fundamental in performing semi joins, allowing for nuanced data filtering.
- Anti joins, in contrast, choose records in the first table where no matching condition is found in the second table.
- Illustrative examples demonstrate how to modify semi joins into anti joins for refined data filtering, such as selecting countries in the Americas founded after 1800.

![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/d7c3e15a-0e0b-4fd1-b02c-b27582ad76c2)
*Addative Join*
![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/411b31cf-1904-4f99-9613-1e7dac7efa7c)
*Semi Join*

![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/5eeec05d-c665-4fa9-b021-a11d69050092)

*Anti Join*
