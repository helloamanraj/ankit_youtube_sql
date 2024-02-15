Question: https://www.youtube.com/watch?v=MQXfhH1d8K0&t=136s

```sql
create table sku 
(
sku_id int,
price_date date ,
price int
);

insert into sku values 
(1,'2023-01-01',10)
,(1,'2023-02-15',15)
,(1,'2023-03-03',18)
,(1,'2023-03-27',15)
```


Solution1: 
```sql
with cte as (
select *, row_number() over (order by sku_id) as mth, str_to_date((concat(year(price_date), "-", row_number() over (order by sku_id), "-", "01")),"%Y-%m-%d") as updated_date from sku
)
,cte2 as (
select sku_id, price_date, price,updated_date, lag(price,1,0) over (order by price ) as lag_price, datediff(updated_date, price_date) as dd from cte
order by updated_date)

select sku_id, updated_date,
case when dd >= 0 then price
when dd <0 then lag_price end as price
from cte2
```