Question: https://www.youtube.com/watch?v=Jo2Ra41QQcU

Solution 1:
```sql
select state,
 max(case when max_city = population then city end) as maxi_city,
 max(case when min_city = population then city end) as maxi_city
 from  (
 select state,city, population,
max(population) over (partition by state) as max_city,
min(population) over (partition by state) as min_city
from city_population 
) as x
group by state
```

Solution 2:

```sql
with cte as (
select state,city, population,
row_number() over (partition by state order by population desc) as rw_1,
row_number() over (partition by state order by population) as rw_2
from city_population 
)

select state, 
max(case when rw_1 = 1 then city end) as max_city,
max(case when rw_2 = 1 then city end) as min_city
from cte
group by state
```