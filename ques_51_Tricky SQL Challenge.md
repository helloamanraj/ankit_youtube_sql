
Question 51:
https://www.youtube.com/watch?v=ACD6J1opmFs&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=51





Tricky SQL Challenge

```sql
create table section_data
(
section varchar(5),
number integer
)
insert into section_data
values ('A',5),('A',7),('A',10) ,('B',7),('B',9),('B',10) ,('C',9),('C',7),('C',9) ,('D',10),('D',3),('D',8)
```


```sql
with cte as (
select *, rank() over(partition by section order by number desc ) as rnk from section_data
)
,
cte2 as
(
select *,
sum(number) over (partition by  section ) as total_sum,
max(number) over (partition by section ) as max_number
from cte
where rnk <3
)
select * from (
select * , dense_rank() over ( order by total_sum desc, max_number desc ) as d_rnk
 from cte2
 ) as A
 where d_rnk <= 2
```