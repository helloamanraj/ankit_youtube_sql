Question 03:
https://www.youtube.com/watch?v=P6kNMyqKD0A&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=3






Complex SQL 3

```sql
create table entries ( 
name varchar(20),
address varchar(20),
email varchar(20),
floor int,
resources varchar(10));

insert into entries 
values ('A','Bangalore','A@gmail.com',1,'CPU'),('A','Bangalore','A1@gmail.com',1,'CPU'),('A','Bangalore','A2@gmail.com',2,'DESKTOP')
,('B','Bangalore','B@gmail.com',2,'DESKTOP'),('B','Bangalore','B1@gmail.com',2,'DESKTOP'),('B','Bangalore','B2@gmail.com',1,'MONITOR');
```

Solution:

```sql
with cte as 
(
select name,
sum(case when floor=1 then 1 else 0 end) as 1st,
sum(case when floor=2 then 1 else 0 end) as 2nd,
count(1) as total_visits,
 group_concat(distinct(resources)) as total_resources from entries group by name 
)
select 
name,
case when 1st>2nd then 1 else 2 end as floor_no,
total_visits,
total_resources
from cte
```