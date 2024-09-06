Question 35:
https://www.youtube.com/watch?v=y-CeVtidYJE&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=35

SQL Interview Question Asked by Udaan:

```sql
create table business_city (
business_date date,
city_id int
)

drop table business_city

insert into business_city
values(cast('2020-01-02' as date),3),(cast('2020-07-01' as date),7),(cast('2021-01-01' as date),3),(cast('2021-02-03' as date),19)
,(cast('2022-12-01' as date),3),(cast('2022-12-15' as date),3),(cast('2022-02-28' as date),12);```


Solution:

```sql

with cte as (
select year(business_date) as year_open,city_id,
rank() over (partition by city_id order by year(business_date)) as rnk from business_city
order by year_open, city_id

)

select year_open, count(city_id) as new_cities from cte
where rnk = 1
group by year_open
```