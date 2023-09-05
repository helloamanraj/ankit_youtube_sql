
Question 56:
https://www.youtube.com/watch?v=C9DGxJKBbb4&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=56

Uplers SQL Interview Problem 

```sql
Create table candidates(
id int primary key,
positions varchar(10) not null,
salary int not null);

insert into candidates values(1,'junior',5000);
insert into candidates values(2,'junior',7000);
insert into candidates values(3,'junior',7000);
insert into candidates values(4,'senior',10000);
insert into candidates values(5,'senior',30000);
insert into candidates values(6,'senior',20000);
```

Solution:
```sql
with cte as (
select *,
sum(salary)over(partition by positions order by salary, id) as running_total,
50000 - sum(salary)over(partition by positions order by salary, id) as remaining_balance
from candidates
),
cte1 as (
select count(*) as total_seniors
from cte
where positions = 'senior'
and remaining_balance > 0
),
cte2 as (
select * , (select coalesce(min(remaining_balance), 50000) from cte where positions = 'senior' and remaining_balance > 0) as balance
from cte
where positions = 'junior'
),
cte3 as (
select count(*) as total_juniors
from cte2
where true
and balance - running_total >= 0
)
select (select total_seniors from cte1) as total_seniors, (select total_juniors from cte3) as total_juniors;
```