Question 33:
https://www.youtube.com/watch?v=Cbm6Hz_Yhwg&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=33

Amazon Interview question:

```sql
CREATE TABLE emp(
  emp_id INT NULL,
  emp_name VARCHAR(50) NULL,
  salary INT NULL,
  manager_id INT NULL,
  emp_age INT NULL,
  dep_id INT NULL,
  dep_name VARCHAR(20) NULL,
  gender VARCHAR(10) NULL
);

insert into emp values(1,'Ankit',14300,4,39,100,'Analytics','Female');
insert into emp values(2,'Mohit',14000,5,48,200,'IT','Male');
insert into emp values(3,'Vikas',12100,4,37,100,'Analytics','Female');
insert into emp values(4,'Rohit',7260,2,16,100,'Analytics','Female');
insert into emp values(5,'Mudit',15000,6,55,200,'IT','Male');
insert into emp values(6,'Agam',15600,2,14,200,'IT','Male');
insert into emp values(7,'Sanjay',12000,2,13,200,'IT','Male');
insert into emp values(8,'Ashish',7200,2,12,200,'IT','Male');
insert into emp values(9,'Mukesh',7000,6,51,300,'HR','Male');
insert into emp values(10,'Rakesh',8000,6,50,300,'HR','Male');
insert into emp values(11,'Akhil',4000,1,31,500,'Ops','Male');
```


Solution:
```sql

WITH cte AS (

  SELECT *,
    RANK() OVER (PARTITION BY dep_id ORDER BY salary DESC) AS max_rank,
    count(*) over (partition by dep_id ) as cnt
  FROM emp
 

)

select * from cte
where max_rank = 3 or (cnt < 3 and max_rank=cnt)
```

Another solution: 
```sql
with cte1 as
(
select *
      ,rank() over ( partition by dep_id order by salary desc) rnk
      ,count(1) over ( partition by dep_id) as total_emp
from emp
)
Select * from cte1
where rnk = case when total_emp>=3 THEN 3
                 when total_emp<3 THEN total_emp```