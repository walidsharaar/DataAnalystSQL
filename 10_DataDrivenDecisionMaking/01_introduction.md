# Summary Introduction to data driven decision making | SQL

### Facts
- Irene Ortner, alongside Professors Bart Baesens and Tim Verdonck, introduces the data-driven decision-making course.
- Refresh basic SQL and introduce advanced SQL techniques for business intelligence, including OLAP extensions like CUBE and ROLLUP.
- The course features a fictional movie rental company, MovieNow, which offers online movie streaming and retains comprehensive data on movies and customer interactions.
- Detailed exploration of 'customers' table in MovieNow's database, including customer ID, personal details, and account creation date.
- Insight into the 'movies' table, covering movie ID, title, genre, runtime, release year, and rental cost.
- Description of the 'renting' table, documenting each rental with identifiers, customer and movie details, ratings, and rental date.
- Examination of the 'actors' table, which lists actor IDs, names, birth years, nationalities, and genders.
- Overview of the 'actsin' table, noting the connections between actors and movies through unique identifiers.
- Use of data for making both short-term operational and long-term strategic decisions, such as movie purchases based on actor popularity or regional expansion based on customer growth analytics.
- Definition and monitoring of key performance indicators like revenue, customer satisfaction, and engagement to inform and refine decision-making processes.
- Encouragement for students to start exploring the database independently, applying what they've learned to real scenarios.

```
 - select only those columns from renting which are needed to calculate the average rating per movie.
SELECT movie_id,
       rating
FROM renting;
```
