Question Link: https://www.youtube.com/watch?v=eMQDHHfUJtU&list=PLBTZqjSKn0IfuIqbMIqzS-waofsPHMS0E&index=19

Question1:
```sql

CREATE TABLE flights 
(
    cid VARCHAR(512),
    fid VARCHAR(512),
    origin VARCHAR(512),
    Destination VARCHAR(512)
);

INSERT INTO flights (cid, fid, origin, Destination) VALUES ('1', 'f1', 'Del', 'Hyd');
INSERT INTO flights (cid, fid, origin, Destination) VALUES ('1', 'f2', 'Hyd', 'Blr');
INSERT INTO flights (cid, fid, origin, Destination) VALUES ('2', 'f3', 'Mum', 'Agra');
INSERT INTO flights (cid, fid, origin, Destination) VALUES ('2', 'f4', 'Agra', 'Kol');
```

Solution:
```sql

with cte as (
select cid, fid, origin, Destination,
rank() over(partition by cid order by fid) as rnk , 
rank() over(partition by cid order by fid desc) as rnk2
from flights
order by fid
) 



select cid,
max(case when rnk = 1 then origin end) as origin,
max(case when rnk2 = 1 then Destination end) as Destination
from cte 
group by cid
```

Question2: 


```sql
CREATE TABLE sales 
(
    order_date date,
    customer VARCHAR(512),
    qty INT
);

INSERT INTO sales (order_date, customer, qty) VALUES ('2021-01-01', 'C1', '20');
INSERT INTO sales (order_date, customer, qty) VALUES ('2021-01-01', 'C2', '30');
INSERT INTO sales (order_date, customer, qty) VALUES ('2021-02-01', 'C1', '10');
INSERT INTO sales (order_date, customer, qty) VALUES ('2021-02-01', 'C3', '15');
INSERT INTO sales (order_date, customer, qty) VALUES ('2021-03-01', 'C5', '19');
INSERT INTO sales (order_date, customer, qty) VALUES ('2021-03-01', 'C4', '10');
INSERT INTO sales (order_date, customer, qty) VALUES ('2021-04-01', 'C3', '13');
INSERT INTO sales (order_date, customer, qty) VALUES ('2021-04-01', 'C5', '15');
INSERT INTO sales (order_date, customer, qty) VALUES ('2021-04-01', 'C6', '10');
```

Solution1:
```sql
with cte as(
select *, month(order_date) as mth, 
row_number() over (partition by customer order by order_date) as rn from sales
)

select mth, count(rn) as new_customer from cte 
where rn = 1
group by mth
```

Solution2:
```sql
select min_date, count(distinct customer) as new_cust from (
select order_date, customer,
min(order_date) over (partition by customer order by order_date) as min_date from sales
) as x
group by min_date
```






