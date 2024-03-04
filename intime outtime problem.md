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


Solution 1: 

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

Solution 2: 

```sql
with cte as  (
select h.emp_id, h.action as ac_in , h.time as intime , j.action as ac_out, j.time as outtime from hospital as h
left join hospital as j on h.emp_id = j.emp_id and j.action = 'out'
where h.action != j.action or h.action = 'in'
)

select distinct emp_id
from cte 
where intime > outtime OR outtime IS NULL

```

Solution 3: 

```sql
with cte as (
select emp_id, action,  max(time) as t1 from hospital
where action ='in'
group by emp_id
)

, cte2 as (
select emp_id,   max(time) as t2 from hospital
group by emp_id
)

select distinct(cte.emp_id) from cte 
inner join cte2 on cte.t1 = cte2.t2
```