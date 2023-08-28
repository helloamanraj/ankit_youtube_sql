Question 31:
https://www.youtube.com/watch?v=fwPk1RXlorQ&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=31

LeetCode Hard SQL problem:

```sql
create table employee
(
emp_id int,
company varchar(10),
salary int
)

insert into employee values (1,'A',2341);
insert into employee values (2,'A',341);
insert into employee values (3,'A',15);
insert into employee values (4,'A',15314);
insert into employee values (5,'A',451);
insert into employee values (6,'A',513);
insert into employee values (7,'B',15);
insert into employee values (8,'B',13);
insert into employee values (9,'B',1154);
insert into employee values (10,'B',1345);
insert into employee values (11,'B',1221);
insert into employee values (12,'B',234);
insert into employee values (13,'C',2345);
insert into employee values (14,'C',2645);
insert into employee values (15,'C',2645);
insert into employee values (16,'C',2652);
insert into employee values (17,'C',65);```

Solution: 

```sql
with cte as (
select *, count(company) over (partition by company) as cnt,
row_number() over (partition by company order by salary) as rw
from employee
)

select company, round(avg(salary),2) as med from cte
where rw in (floor((cnt+1)*1.0/2) , ceiling((cnt+1)*1.0/2))
group by company

example of: floor(5.7) = 5 (The largest integer less than or equal to 5.7 is 5.)
example of : ceiling(5.7) = 6 (The smallest integer greater than or equal to 5.7 is 6.)
```

Another Solution: 

```sql
with cte as (

select *,
abs(cast(row_number() over (partition by company order by salary desc) as signed) -
cast(row_number() over (partition by company order by salary ) as signed )) as abs_diff
from employee
order by company, salary
)

select avg(salary) from cte
where abs_diff <= 1
group by company```