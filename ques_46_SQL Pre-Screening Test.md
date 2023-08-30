Question 46:
https://www.youtube.com/watch?v=QHwHS4AMmQM&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=46






SQL Pre-Screening Test:
```sql
create table tbl_orders (
order_id integer,
order_date date
)

insert into tbl_orders
values (1,'2022-10-21'),(2,'2022-10-22'),
(3,'2022-10-25'),(4,'2022-10-25')

select * from tbl_orders

create table  tbl_orders_copy as select * from tbl_orders

insert into tbl_orders
values (5,'2022-10-26'),(6,'2022-10-26')
```

Solution:

```sql

with cte as (
SELECT t1.order_id, ifnull(t2.order_id, 'D') as flag
FROM tbl_orders t1
LEFT JOIN tbl_orders_copy t2 ON t1.order_id = t2.order_id

UNION

SELECT t2.order_id, ifnull(t1.order_id, 'I')as flag
FROM tbl_orders as t1
RIGHT JOIN tbl_orders_copy t2 ON t1.order_id = t2.order_id


)


select order_id,flag
from cte
where flag in ('D', 'I')
```