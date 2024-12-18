Question 15 & 16:
https://www.youtube.com/watch?v=i_ljK9gmstY&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=14

Customer Retention and Churn Analysis:

```sql

create table transactions(
order_id int,
cust_id int,
order_date date,
amount int
);
delete from transactions;
insert into transactions values 
(1,1,'2020-01-15',150)
,(2,1,'2020-02-10',150)
,(3,2,'2020-01-16',150)
,(4,2,'2020-02-25',150)
,(5,3,'2020-01-10',150)
,(6,3,'2020-02-20',150)
,(7,4,'2020-01-20',150)
,(8,5,'2020-02-20',150)
;

```
Customer Retention Solution 1: 

```sql

with cte_1 as 
(
select *,lag(month(order_date)) over (partition by cust_id order by month(order_date)) as last_date from transactions
)
select month (order_date) month ,count(case when last_date= 1 then cust_id end ) as customer_retained from cte_1 
group by month (order_date)

```

Solution 2:
```sql
with cte as (
select order_id, cust_id, month(order_date) as mth, lag(month(order_date),1) over (partition by cust_id order by order_id) as ld
,amount from transactions
)

select  mth,  sum(case when mth - ld = 1 then 1 else 0 end) as repeat_cust 
from cte
group by mth
```

Churn Analysis Solution: 

```sql

with cte as (
select *,month(order_date) as mth
,lead(month(order_date)) over (partition by cust_id order by month(order_date)) as last_date from transactions
)


SELECT mth,
       count(CASE WHEN (mth - last_date) is null THEN cust_id END) AS churn_customer
FROM cte
group by mth



```