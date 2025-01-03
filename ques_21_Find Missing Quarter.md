Question 21:
https://www.youtube.com/watch?v=cGP5Tm2gVdQ&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=21

Find Missing Quarter:

```sql


CREATE TABLE STORES (
Store varchar(10),
Quarter varchar(10),
Amount int);

INSERT INTO STORES (Store, Quarter, Amount)
VALUES ('S1', 'Q1', 200),
('S1', 'Q2', 300),
('S1', 'Q4', 400),
('S2', 'Q1', 500),
('S2', 'Q3', 600),
('S2', 'Q4', 700),
('S3', 'Q1', 800),
('S3', 'Q2', 750),
('S3', 'Q3', 900);
```

Solution_1:

```sql

with cte as (
SELECT 
    store,
    Quarter,
    SUBSTRING(Quarter, 1, 1) AS qua_group,
    SUBSTRING(Quarter, 2,2) AS qua_number,
    10 as qua_sum,
    sum(SUBSTRING(Quarter, 2,2)) over (partition by store order by store) as sum_qua_num
FROM stores
)

select distinct store,  concat('Q', qua_sum - sum_qua_num) as miss_quater from cte
```

Solution_2:


```sql
WITH RECURSIVE cte AS (
    (SELECT DISTINCT store, 1 AS q_no FROM stores)
    UNION ALL
    (SELECT store, q_no + 1 AS q_no FROM cte
    WHERE q_no < 4)
)
, cte2 as (
SELECT *, concat('Q',q_no) as q_update FROM cte
)

select  c.store as store, c.q_update as quater from cte2 as c
left join stores as s on c.q_update = s.quarter and c.store = s.store
where s.store is null
```
Solution 3: 

```sql 

 with cte as 
 (select store, Quarter, Right(Quarter,1) as nm, row_number() over (partition by store) as rw
 from stores
 )

select distinct store, Concat('Q', (10 - sum(nm) over (partition by store))) as sm from cte
 
```








```