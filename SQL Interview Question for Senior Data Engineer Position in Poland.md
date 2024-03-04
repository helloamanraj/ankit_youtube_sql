Question Link: https://www.youtube.com/watch?v=TvqKpz9RO-A&list=PLBTZqjSKn0IfuIqbMIqzS-waofsPHMS0E&index=5

```sql
CREATE TABLE emp_salary
(
    emp_id INTEGER  NOT NULL,
    name NVARCHAR(20)  NOT NULL,
    salary NVARCHAR(30),
    dept_id INTEGER
)
;


INSERT INTO emp_salary
(emp_id, name, salary, dept_id)
VALUES(101, 'sohan', '3000', '11'),
(102, 'rohan', '4000', '12'),
(103, 'mohan', '5000', '13'),
(104, 'cat', '3000', '11'),
(105, 'suresh', '4000', '12'),
(109, 'mahesh', '7000', '12'),
(108, 'kamal', '8000', '11');
```

Solution1: 

```sql
with cte as (
select *, rank() over (partition by dept_id order by salary desc ) as rnk from emp_salary 
)

select emp_id, name, salary, dept_id from (select *, 
lead(rnk,1, rnk) over (partition by dept_id order by rnk) as ld, count(dept_id) over (partition by dept_id) as cnt 
from cte
) as x
where cnt > 1 and rnk = ld

```

Solution2: 
```sql
with cte as (
select *, dense_rank() over (partition by dept_id order by salary) as dn_rnk
from emp_salary
)

select emp_id, name, salary, dept_id from (select *, count(dn_rnk) over (partition by dept_id, dn_rnk) as cnt from cte )
as x where cnt = 2
 ```

 Solution3: 

 ```sql
 with cte as (
select salary, dept_id from emp_salary
group by salary, dept_id 
having count(1) > 1
)

select * from cte as c
left join emp_salary as e_sal on c.dept_id = e_sal.dept_id and c.salary = e_sal.salary
 
 ```