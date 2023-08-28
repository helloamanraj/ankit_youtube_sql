Question 30:
https://www.youtube.com/watch?v=e-I9SxbLky8&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=30

LeetCode Hard SQL problem:

```sql
create table players_location
(
name varchar(20),
city varchar(20)
)

insert into players_location
values ('Sachin','Mumbai'),('Virat','Delhi') , ('Rahul','Bangalore'),('Rohit','Mumbai'),('Mayank','Bangalore');
```


Solution:

```sql
with cte as (

select *,
row_number() over (partition by city order by name) as rw from
players_location

)

select
max(case when city = 'Mumbai' then name end) as Mumbai,
max(case when city = 'Delhi' then name end) as Delhi,
max(case when city = 'Bangalore' then name end) as Bangalore
from cte
group by rw
```