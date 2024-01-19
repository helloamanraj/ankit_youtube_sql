question link: https://www.youtube.com/watch?v=oGYinDMDfnA&list=PLBTZqjSKn0IfuIqbMIqzS-waofsPHMS0E&index=3


```sql
create table hospital ( emp_id int
, action varchar(10)
, time datetime);

insert into hospital values ('1', 'in', '2019-12-22 09:00:00');
insert into hospital values ('1', 'out', '2019-12-22 09:15:00');
insert into hospital values ('2', 'in', '2019-12-22 09:00:00');
insert into hospital values ('2', 'out', '2019-12-22 09:15:00');
insert into hospital values ('2', 'in', '2019-12-22 09:30:00');
insert into hospital values ('3', 'out', '2019-12-22 09:00:00');
insert into hospital values ('3', 'in', '2019-12-22 09:15:00');
insert into hospital values ('3', 'out', '2019-12-22 09:30:00');
insert into hospital values ('3', 'in', '2019-12-22 09:45:00');
insert into hospital values ('4', 'in', '2019-12-22 09:45:00');
insert into hospital values ('5', 'out', '2019-12-22 09:40:00');

```


Solution: 

```sql
WITH cte AS (
  SELECT emp_id, 
  coalesce(max(case when action = 'in' then time end), '00:00:00') as intime, 
  coalesce(min(case when action = 'out' then time end), '00:00:00') as outime
  FROM hospital
  GROUP BY emp_id
)


SELECT emp_id from cte 
where intime > outime 
```