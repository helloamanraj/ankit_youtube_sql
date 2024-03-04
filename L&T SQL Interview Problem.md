Question: https://www.youtube.com/watch?v=7W7B0y5WsaQ&list=PLBTZqjSKn0IfuIqbMIqzS-waofsPHMS0E&index=11

```sql
create table employee 
(
emp_name varchar(10),
dep_id int,
salary int
);


insert into employee values 
('Siva',1,30000),('Ravi',2,40000),('Prasad',1,50000),('Sai',2,20000)

```

Solution1: 

```sql
with cte as (
select *,
max(salary) over (partition by dep_id order by salary desc) as mx ,
min(salary) over (partition by dep_id order by salary) as mn
from employee
)

select dep_id, max(case when salary = mx then emp_name else null end) as maxim,
max(case when salary = mn then emp_name else null end) as mini
from cte 
group by dep_id
```

Solution2: 

```sql
with cte as (
select *, row_number() over (partition by dep_id order by salary desc) as high_sal, 
row_number() over (partition by dep_id order by salary ) as low_sal
from employee
)

select dep_id, emp_name from cte
where high_sal = 1 or low_sal =1 
```