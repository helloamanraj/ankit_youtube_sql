Question 13: https://www.youtube.com/watch?v=9Kh7EnZlhUg&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=13

Data Science SQL Interview Question:

```sql 


create table orders
(
order_id int,
customer_id int,
product_id int
)

insert into orders VALUES 
(1, 1, 1),
(1, 1, 2),
(1, 1, 3),
(2, 2, 1),
(2, 2, 2),
(2, 2, 4),
(3, 1, 5);

create table products (
id int,
name varchar(10)
);


insert into products VALUES 
(1, 'A'),
(2, 'B'),
(3, 'C'),
(4, 'D'),
(5, 'E');


```

Solution: 

```sql



with cte as (
select *, lead(name,1) over (partition by customer_id, order_id order by order_id) as led,
 lead(name,2) over (partition by customer_id, order_id order by order_id) as led2
from orders as o 
inner join products as p on o.product_id = p.id
)
, cte2 as (
select order_id, customer_id,  GROUP_CONCAT(name, led) grp1 , group_concat(name, led2) as grp2 from cte
group by order_id, customer_id, product_id
)

SELECT grp, COUNT(*) AS count
FROM (
    SELECT grp1 AS grp FROM cte2 WHERE grp1 IS NOT NULL
    UNION ALL
    SELECT grp2 AS grp FROM cte2 WHERE grp2 IS NOT NULL
) AS subquery
GROUP BY grp;

```





















```