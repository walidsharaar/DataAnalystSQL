## Summary LEFT and RIGHT JOINs | SQL

This chapter delves into outer joins—LEFT and RIGHT JOINs—contrasting them with INNER JOINs. LEFT JOIN retains all records from the left table and matches those from the right table, while RIGHT JOIN retains all from the right table.

## Facts
- In INNER JOIN, only matching records from both tables are included in the result.
  ![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/00951eb3-62e2-4d78-8f1f-2389bdc08503)
*Inner Join*
- LEFT JOIN keeps all left_table records and matches those from the right_table. Unmatched values from right_table show as null.
- Diagrams illustrate the differences between INNER and LEFT JOIN outcomes.
- LEFT JOIN syntax mirrors INNER JOIN, but retrieves unmatched values from the right table.
![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/8252b52a-7b88-49a0-85e2-39aaf6a3bc55)
![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/22f6aae9-da3d-4c91-b5d6-7da9c7e374f3)
*left join*
- RIGHT JOIN retains all right_table records, showing null for unmatched left_table entries.
- RIGHT JOIN syntax mirrors LEFT JOIN, but focuses on retaining unmatched right_table values.
  ![image](https://github.com/walidsharaar/DataAnalystSQL/assets/29350894/731856c8-3418-47fe-a0b1-785c0b0e79e6)
*Right join*
- Concrete examples using world leaders showcase the differences between LEFT and RIGHT JOIN.
- RIGHT JOIN is less common due to its interchangeability with a rearranged LEFT JOIN, which feels more intuitive in query construction.
