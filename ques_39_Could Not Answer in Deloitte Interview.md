Question 39:
https://www.youtube.com/watch?v=Xh0EevUOWF0&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=39


Could Not Answer in Deloitte Interview:

```sql
create table brands
(
category varchar(20),
brand_name varchar(20)
);
insert into brands values
('chocolates','5-star')
,(null,'dairy milk')
,(null,'perk')
,(null,'eclair')
,('Biscuits','britannia')
,(null,'good day')
,(null,'boost');
```


Solution:

```sql

with cte as (

select *, row_number() over (order by (select null)) as rn
from brands
)

select min(category) over (order by rn rows between unbounded preceding and current row) as cat,
brand_name
from cte 
```

Another Solution:
```sql
with cte as
(
select *, row_number () over() as rn from brands

)

select
(case when category is null then ( select category from cte as c1 where c1.rn < c2.rn and category is not null order by  rn desc limit 1)
else category end)
category, brand_name
from cte as c2
```