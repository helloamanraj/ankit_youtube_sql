Question 47:
https://www.youtube.com/watch?v=eayyD51fIVY&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=47




Uber Very Interesting SQL:

```sql
create table drivers(id varchar(10), start_time time, end_time time, start_loc varchar(10), end_loc varchar(10));
insert into drivers values('dri_1', '09:00', '09:30', 'a','b'),('dri_1', '09:30', '10:30', 'b','c'),('dri_1','11:00','11:30', 'd','e');
insert into drivers values('dri_1', '12:00', '12:30', 'f','g'),('dri_1', '13:30', '14:30', 'c','h');
insert into drivers values('dri_2', '12:15', '12:30', 'f','g'),('dri_2', '13:30', '14:30', 'c','h');```


Solution: 
```sql
with cte as (
select *,
CASE WHEN EXISTS (SELECT * FROM drivers as t2 WHERE t1.start_loc = t2.end_loc and t1.id = t2.id) THEN 1 ELSE 0 END AS matched
from drivers as t1
)
select id,count(id) as total_rides, max(matched) as profitable_rides from cte
group  by id


another solution:

select * from drivers

with cte as (
Select *, lead(start_loc,1) over(partition by id order by start_time) as next_start,
case when lead(start_loc,1) over(partition by id order by start_time) = end_loc then 1 else 0 end as profit_count
from drivers
)

select id, count(id) as total_trips, sum(profit_count) as profitable_trips
from cte
group by id```