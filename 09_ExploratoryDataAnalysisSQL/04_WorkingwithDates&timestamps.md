## Summary Date/time types and formats | SQL
This segment covers the fundamentals of handling date/time data in databases, highlighting types, formats, and operations like addition and subtraction.
### Facts
- Date/time data encompasses columns storing dates or times, with main types being date, timestamp, and intervals.
- Two primary types are dates (year, month, day) and timestamps (date plus time down to microseconds).
- Intervals represent durations and are often results of subtracting one date/time from another, defaulting to days and time display.
- Multiple formats for recording dates and times exist, leading to potential ambiguity in data representation.
- ISO 8601 standardizes date/time recording, ordering units from largest to smallest and using fixed digit counts.
- Timezones complicate datetime data, with Postgres using UTC and allowing for timezone offsets in timestamps.
- Dates and timestamps can be compared using conventional operators, and the current timestamp is obtainable via the now function.
- Subtraction between dates results in an interval type.
- Dates can have days added directly, while more complex intervals require specifying durations in terms of years, days, and minutes, among others.

```
-- Count requests created on January 31, 2017 and count the number of Evanston 311 requests created on January 31, 2017 by casting date_created to a date.
SELECT count(*) 
  FROM evanston311
 WHERE date_created::date = '2017-01-31'
-- Count requests created on February 29, 2016
SELECT count(*)
  FROM evanston311 
 WHERE date_created >= '2016-02-29' 
   AND date_created < '2016-03-01';

-- Count requests created on March 13, 2017 and upper bound by adding 1 to the lower bound.
SELECT count(*)
  FROM evanston311
 WHERE date_created >= '2017-03-13'
   AND date_created < '2017-03-13'::date + 1;

-- Subtract the min date_created from the max
SELECT max(date_created) - min(date_created)
  FROM evanston311;

--Compute the average difference between the completion timestamp and the creation timestamp by category and order the results with 
the largest average time to complete the request first..
SELECT category, 
       avg(date_completed - date_created) AS completion_time
  FROM evanston311
 GROUP BY category
 ORDER BY completion_time DESC;
```


## Summary Date/time components and aggregation 

Date and time manipulation in databases involves extracting specific components or truncating values to aggregate data effectively, with functions and standards detailed in the Postgres documentation.
### Facts
- Extracting components of date/time data allows for detailed analysis and aggregation.
- Common date/time fields include century, decade, year, month, day, hour, minute, second, and week number, following ISO 8601 standards.
- The date_part and extract functions are used to extract specific fields from dates or timestamps, with slight differences in syntax.
- Extracting fields is beneficial for analyzing how data varies across different units of time, such as monthly sales trends over years.
- The date_trunc function truncates dates to a specified precision, keeping larger units and setting smaller ones to zero or one, useful for aggregating data by larger time units.
- Truncating dates helps in understanding trends over larger units of time by summarizing data associated with specific periods, like monthly sales trends.

  ```
  --How many requests are created in each of the 12 months during 2016-2017?
  SELECT date_part('month', date_created) AS month, 
       count(*)
  FROM evanston311
  WHERE date_created >= '2016-01-01'
   AND date_created < '2018-01-01'
  GROUP BY month;
  -- What is the most common hour of the day for requests to be created?
  SELECT date_part('hour', date_created) AS hour,
       count(*)
  FROM evanston311
  GROUP BY hour
  ORDER BY count DESC
  LIMIT 1;
  -- Count requests completed by hour
  SELECT date_part('hour', date_completed) AS hour,
       count(*)
  FROM evanston311
  GROUP BY hour
  ORDER BY hour;
  --Select the name of the day of the week the request was created (date_created) as day then select the mean time between the request completion (date_completed) and request creation as duration.Group by day (the name of the day of the week) and the integer value for the day of the week (use a function) and order by the integer value of the day of the week using the same function used in GROUP BY.
  SELECT to_char(date_created, 'day') AS day,
  avg(date_completed - date_created) AS duration
  FROM evanston311
  GROUP BY day, EXTRACT(DOW FROM date_created)
  ORDER BY EXTRACT(DOW FROM date_created);
  -- Write a subquery to count the number of requests created per day and select the month and average count per month from the daily_count subquery.
  SELECT date_trunc('month', day) AS month,
       avg(count)
  FROM (SELECT date_trunc('day', date_created) AS day,
               count(*) AS count
          FROM evanston311
         GROUP BY day) AS daily_count
  GROUP BY month
  ORDER BY month;

  ```


## Summary Aggregating with date/time series | SQL

Using the generate_series function with date/time data enables the inclusion of periods with no observations in aggregated data results, enhancing data analysis accuracy.
### Facts
- When aggregating date/time series data, periods without observations can be included by generating a series of timestamps.
- The generate_series function works with date/time data by expecting timestamps and an interval, allowing for flexible data series creation.
- An example of generating a series with an interval of hours demonstrates that the series ends at a specified maximum timestamp.
- To avoid unexpected results, it's advisable to generate series starting from the beginning of a month or year, not the end.
- Correctly generating a series for the last day of each month involves starting from the beginning of each month and subtracting one day.
- Normal aggregation might miss out on representing units of time with no observations, like sales data by the hour.
- Aggregating with a series introduces rows for missing hours in data, ensuring a comprehensive representation of time-based observations.
- The aggregation result with series shows all desired hours, including those with zero sales, preventing overlooked data.
- For non-standard intervals, bins can aggregate data, such as counting sales in three-hour intervals.
- The bin aggregation result reveals sales counts for each specified interval, offering a nuanced view of data distribution.

```
--Write a subquery using generate_series() to get all dates between the min() and max() date_created in evanston311 then write another subquery to select all values of date_created as dates from evanston311.
--Both subqueries should produce values of type date (look for the ::) and elect dates (day) from the first subquery that are NOT IN the results of the second subquery. |
SELECT day
  FROM (SELECT generate_series(min(date_created),
                               max(date_created),
                               '1 day')::date AS day
          FROM evanston311) AS all_dates      
 WHERE day NOT IN 
       (SELECT date_created::date
          FROM evanston311);

SELECT generate_series('2016-01-01',  
                       '2018-01-01',  
                       '6 months'::interval) AS lower,
       generate_series('2016-07-01',  
                       '2018-07-01',  
                       '6 months'::interval) AS upper;

SELECT day, count(date_created) AS count
  FROM (SELECT generate_series('2016-01-01',  -- series start date
                               '2018-06-30',  -- series end date
                               '1 day'::interval)::date AS day) AS daily_series
       LEFT JOIN evanston311
       -- match day from above (which is a date) to date_created
       ON day = date_created::date
 GROUP BY day;
WITH bins AS (
	 SELECT generate_series('2016-01-01',
                            '2018-01-01',
                            '6 months'::interval) AS lower,
            generate_series('2016-07-01',
                            '2018-07-01',
                            '6 months'::interval) AS upper),
     daily_counts AS (
     SELECT day, count(date_created) AS count
       FROM (SELECT generate_series('2016-01-01',
                                    '2018-06-30',
                                    '1 day'::interval)::date AS day) AS daily_series
            LEFT JOIN evanston311
            ON day = date_created::date
      GROUP BY day)
SELECT lower, 
       upper, 
       percentile_disc(0.5) WITHIN GROUP (ORDER BY count) AS median
  FROM bins
       LEFT JOIN daily_counts
       ON day >= lower
          AND day < upper
 GROUP BY lower, upper
 ORDER BY lower;

--Generate a series of dates from 2016-01-01 to 2018-06-30 and join the series to a subquery to count the number of requests created per day. thenUse date_trunc() to get months from date, which has all dates, NOT day and use coalesce() to replace NULL count values with 0. Compute the average of this value.
WITH all_days AS 
     (SELECT generate_series('2016-01-01',
                             '2018-06-30',
                             '1 day'::interval) AS date),
     daily_count AS 
     (SELECT date_trunc('day', date_created) AS day,
             count(*) AS count
        FROM evanston311
       GROUP BY day)
SELECT date_trunc('month', date) AS month,
       avg(coalesce(count, 0)) AS average
  FROM all_days
       LEFT JOIN daily_count
       ON all_days.date=daily_count.day
 GROUP BY month
```

## Summary Time between events | SQL
The lead and lag functions are used to calculate the time between events in a database, specifically demonstrating how to find the average time between sales and changes in a time series.
### Facts
- The lead and lag functions offset ordered column values by one row to calculate differences between events.
- These functions require specifying an order for the rows, as database tables do not inherently have one.
- By ordering sales dates from oldest to newest, the lag function can be used to compute the gap between each sale.
- The average time between sales is calculated as 32 minutes and 15 seconds, requiring a subquery due to aggregation restrictions.
- Lead and lag are applicable beyond date/time data, enabling analyses of changes in quantities or any time series variable.
  ```
  ```
  
