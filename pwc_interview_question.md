Question: https://www.youtube.com/watch?v=8nfzv1XI1Ic&t=17s

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