Question: https://www.youtube.com/watch?v=8nfzv1XI1Ic&t=17s

Table:
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

Solution:

```sql
with cte as (
select *, lag(revenue,1,0) over (partition by company order by year)  as lg,
revenue - lag(revenue,1,0) over (partition by company order by year) as amt 
from company_revenue 
)
,
cte2 as ( 
SELECT company, 
sum(case when (amt) > 0 then 1 else 0 end ) as positve_amt ,
count(company) as grp_cmpy
FROM cte
GROUP BY company
)

select company from cte2
where grp_cmpy = positve_amt
```