Question 02:
https://www.youtube.com/watch?v=qyAgWL066Vo&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=1



find new and repeat customers

```sql
create table customer_orders (
order_id integer,
customer_id integer,
order_date date,
order_amount integer
);

insert into customer_orders values(1,100,cast('2022-01-01' as date),2000),(2,200,cast('2022-01-01' as date),2500),(3,300,cast('2022-01-01' as date),2100)
,(4,100,cast('2022-01-02' as date),2000),(5,400,cast('2022-01-02' as date),2200),(6,500,cast('2022-01-02' as date),2700)
,(7,100,cast('2022-01-03' as date),3000),(8,400,cast('2022-01-03' as date),1000),(9,600,cast('2022-01-03' as date),3000)

```

Solution 1:

```sql
with cte as (
select *, row_number() over (partition by customer_id order by order_date ) as rw
from customer_orders
)


select order_date, sum(case when rw = 1 then 1 else 0 end) as first_visit,
sum(case when rw != 1 then 1 else 0 end) as repeat_cust
from cte
group by order_date
```

Solution 2

```sql
with cte as (
select customer_id, min(order_date) as first_order_date 
from customer_orders
group by customer_id
)

select  order_date,
sum(case when order_date = first_order_date then 1 else 0 end) as new_customer,
sum(case when order_date != first_order_date then 1 else 0 end) as repeat_customer
from cte as c inner join customer_orders as co on co.customer_id = c.customer_id 
group by order_date
```