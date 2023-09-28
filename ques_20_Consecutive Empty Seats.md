Question 20:
https://www.youtube.com/watch?v=F9Otofceer0&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=20

Consecutive Empty Seats:

```sql

create table bms (seat_no int ,is_empty varchar(10));
insert into bms values
(1,'N')
,(2,'Y')
,(3,'N')
,(4,'Y')
,(5,'Y')
,(6,'Y')
,(7,'N')
,(8,'Y')
,(9,'Y')
,(10,'Y')
,(11,'Y')
,(12,'N')
,(13,'Y')
,(14,'Y');
```

Solution_1 by using row number: 

```sql

with cte as (
select *, row_number() over (partition by is_empty order by seat_no) as rw, 
seat_no - row_number() over (partition by is_empty order by seat_no) as diff
from bms
)
,
cte2 as 
(select seat_no, count(diff) over (partition by diff) as empt_seat_cons   from cte 
where is_empty = 'Y'
)
select seat_no from cte2 
where empt_seat_cons >= 3
```

Solution_2 by using lead and lag:

```sql



select seat_no from (
SELECT *, 
   lag(is_empty, 2) OVER (ORDER BY seat_no) AS prev_2 ,
   lag(is_empty, 1) OVER (ORDER BY seat_no) AS prev_1,
   lead(is_empty, 1) OVER (ORDER BY seat_no) AS next_1,
   lead(is_empty, 2) OVER (ORDER BY seat_no) AS next_2
FROM bms) as x
where (is_empty = 'Y' and prev_2 = 'Y' and prev_1 = 'Y') or
      (is_empty = 'Y' and prev_1 = 'Y' and next_1 = 'Y') or
      (is_empty = 'Y' and next_2 = 'Y' and next_1 = 'Y')
order by seat_no
```

Solution_2 Best Solution:

```sql
with cte as (
SELECT *, 
   sum(case when is_empty = 'Y' then 1 else 0 end) OVER (ORDER BY seat_no rows between 2 preceding and current row) AS prev_1 ,
   sum(case when is_empty = 'Y' then 1 else 0 end) OVER (ORDER BY seat_no  rows between 1 preceding and 1 following) AS current_row,
   sum(case when is_empty = 'Y' then 1 else 0 end) OVER (ORDER BY seat_no rows between current row and 2 following) AS next_row
FROM bms
)

select seat_no from cte
where prev_1 = 3 or current_row= 3 or next_row =3
```