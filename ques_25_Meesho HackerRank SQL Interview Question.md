Question 25:
https://www.youtube.com/watch?v=B09xhslOvxw&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=25

Meesho HackerRank SQL Interview Question:

```sql


create table products
(
product_id varchar(20) ,
cost int
);
insert into products values ('P1',200),('P2',300),('P3',500),('P4',800);

create table customer_budget
(
customer_id int,
budget int
);

insert into customer_budget values (100,400),(200,800),(300,1500);
```

Solution:

```sql
with cte as (
select *, sum(cost) over (order by cost rows between unbounded preceding and current row) as cum
from products
)

select customer_id, budget, count(*),
group_concat(product_id)  
from cte as c
join customer_budget on c.cum <= budget
group by customer_id, budget


```