Question 36:
https://www.youtube.com/watch?v=e4IILSHtKl4&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=36

PharmEasy SQL Interview Question:


```sql
create table movie(
seat varchar(50),occupancy int
);
insert into movie values('a1',1),('a2',1),('a3',0),('a4',0),('a5',0),('a6',0),('a7',1),('a8',1),('a9',0),('a10',0),
('b1',0),('b2',0),('b3',0),('b4',1),('b5',1),('b6',1),('b7',1),('b8',0),('b9',0),('b10',0),
('c1',0),('c2',1),('c3',0),('c4',1),('c5',1),('c6',0),('c7',1),('c8',0),('c9',0),('c10',1);
```

Solution:

```sql
with cte as (
select *, substring(seat, 1,1) as rw_in, cast(substring(seat, 2,2) as signed) as seat_no,
row_number() over (partition by substring(seat, 1,1)  order by cast(substring(seat, 2,2) as signed) ) as rw_no,
CAST(SUBSTRING(seat, 2, 2) AS SIGNED) -
       (ROW_NUMBER() OVER (PARTITION BY SUBSTRING(seat, 1, 1) ORDER BY CAST(SUBSTRING(seat, 2, 2) AS SIGNED))) AS diff
FROM movie
where occupancy = 0
)

select seat
from
(select seat,
count(diff) over(partition by rw_in, diff) as cnt
from cte) A
where cnt=4
```