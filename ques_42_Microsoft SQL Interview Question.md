Question 42:
https://www.youtube.com/watch?v=KLqRHJ-Eg2s&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=42



Microsoft SQL Interview Question:

```sql
create table candidates (
emp_id int,
experience varchar(20),
salary int
);
delete from candidates;
insert into candidates values
(1,'Junior',10000),(2,'Junior',15000),(3,'Junior',40000),(4,'Senior',16000),(5,'Senior',20000),(6,'Senior',50000);
```

Solution:
```sql

with cte as (
select * ,
sum(salary) over(partition by experience order by salary asc rows between unbounded preceding and current row) as running_sal
from candidates
)
, ex_senior as (
select * from cte
where experience = 'Senior' and running_sal < 70000
)

select * from cte
where experience = 'Junior' and running_sal < 70000- (select sum(salary) from ex_senior)
union all
select * from ex_seniors```


Another Solution:
```sql
with cte as
                (  SELECT *  
                                  ,SUM(salary) over ( partition by experience order by salary) as runing_sum
                   FROM candidates
               )
, team_budget as
              (   SELECT  70000 as budget )
, cte2 as
              ( SELECT    *
                                 , SUM(c.salary) over (  order by c.experience Desc , c.salary) as runing_sum1  
                FROM cte c JOIN team_budget tb
                         ON c.runing_sum <=tb.budget
               )

SELECT * FROM cte2 WHERE runing_sum1<=budget```