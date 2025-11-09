### Assume you're given tables with information on Snapchat users, including their ages and time spent sending and opening snaps.

### Write a query to obtain a breakdown of the time spent sending vs. opening snaps as a percentage of total time spent on these activities grouped by age group. Round the percentage to 2 decimal places in the output.

__Notes:__

*Calculate the following percentages:*
*time spent sending / (Time spent sending + Time spent opening)*
*Time spent opening / (Time spent sending + Time spent opening)*
*To avoid integer division in percentages, multiply by 100.0 and not 100.*

### Solution 

```
WITH SortedBuckets AS (
  SELECT 
    age_bucket, 
    SUM(CASE WHEN acts.activity_type = 'open' THEN acts.time_spent ELSE 0 END) AS open_timespent,
    SUM(CASE WHEN acts.activity_type = 'send' THEN acts.time_spent ELSE 0 END) AS send_timespent,
    SUM(acts.time_spent) AS total_timespent
  FROM age_breakdown 
  JOIN activities AS acts ON age_breakdown.user_id = acts.user_id
  WHERE acts.activity_type IN ('open', 'send')
  GROUP BY age_bucket
)
SELECT 
  age_bucket,
  round(100.0 * open_timespent / total_timespent, 2) AS open_perc,
  round(100.0 * send_timespent / total_timespent, 2) AS send_perc
FROM SortedBuckets;
```