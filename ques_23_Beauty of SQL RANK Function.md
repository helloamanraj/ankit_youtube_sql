Question 23:
https://www.youtube.com/watch?v=7okRHS6WL0c&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=23

Beauty of SQL RANK Function:


```sql


create table covid(city varchar(50),days date,cases int);
delete from covid;
insert into covid values('DELHI','2022-01-01',100);
insert into covid values('DELHI','2022-01-02',200);
insert into covid values('DELHI','2022-01-03',300);

insert into covid values('MUMBAI','2022-01-01',100);
insert into covid values('MUMBAI','2022-01-02',100);
insert into covid values('MUMBAI','2022-01-03',300);

insert into covid values('CHENNAI','2022-01-01',100);
insert into covid values('CHENNAI','2022-01-02',200);
insert into covid values('CHENNAI','2022-01-03',150);

insert into covid values('BANGALORE','2022-01-01',100);
insert into covid values('BANGALORE','2022-01-02',300);
insert into covid values('BANGALORE','2022-01-03',200);
insert into covid values('BANGALORE','2022-01-04',400);

```

Solution 1:


```sql
with cte as (
select *,
cast(rank() over (partition by city order by days ) as signed) as rnk_1,
cast(rank() over (partition by city order by cases ) as signed) as rnk_2
from covid
)
, cte2 as (
select city , (rnk_1 - rnk_2 )as diff from cte)

select city from cte2
group by city
having count(distinct diff) = 1 and max(diff) = 0
```

Solution 2:

```sql
with cte1 as(
select *,
lag(cases,1,0) over(partition by city order by days) as prev
from covid
)
select city
from cte1
group by city
having SUM(case when cases>prev then 0 else 1 end)=0
```