Question 10:
https://www.youtube.com/watch?v=WrToXXN7Jb4&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=10

Tricky SQL Logic:

```sql

create table tasks (
date_value date,
state varchar(10)
);

insert into tasks  values ('2019-01-01','success'),('2019-01-02','success'),('2019-01-03','success'),('2019-01-04','fail')
,('2019-01-05','fail'),('2019-01-06','success')

```

Solution: 
```sql
WITH cte AS (
  SELECT
    *,
    LAG(state, 1, state) OVER (ORDER BY date_value) AS prev_state
  FROM tasks
)

SELECT
  MIN(date_value) AS start_date,
  MAX(date_value) AS end_date,
  state
FROM (
  SELECT
    *,
    SUM(CASE WHEN state = prev_state THEN 0 ELSE 1 END) OVER (ORDER BY date_value) AS group_id
  FROM cte
) grouped
GROUP BY
  group_id, state
ORDER BY
  start_date;```