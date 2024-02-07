Question: https://www.youtube.com/watch?v=8nfzv1XI1Ic&list=PLBTZqjSKn0IfuIqbMIqzS-waofsPHMS0E&index=16

```sql
create table company_revenue 
(
company varchar(100),
year int,
revenue int
)

insert into company_revenue values 
('ABC1',2000,100),('ABC1',2001,110),('ABC1',2002,120),('ABC2',2000,100),('ABC2',2001,90),('ABC2',2002,120)
,('ABC3',2000,500),('ABC3',2001,400),('ABC3',2002,600),('ABC3',2003,800);
```

Solution1: 
```sql

with cte as (
select *, rank() over (partition by company order by year) as rk1, 
rank() over (partition by company order by revenue) as rk2
from company_revenue
)

select max(company) as company from cte
where rk1 != rk2
```

Solution2:
```sql
with cte as (
select *, revenue - lag(revenue,1,0) over (partition by company order by year) as diff,
count(*) over (partition by company) as cnt1 
from company_revenue 
)

select company, cnt1 from cte
where diff > 0 
group by company
having cnt1 = count(1) 
```





