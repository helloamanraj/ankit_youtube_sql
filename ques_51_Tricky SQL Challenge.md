
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


Another Solution:
```sql
WITH agg_data AS(
SELECT section,
min(number) AS min_no,
max(number) AS max_no,
DENSE_RANK()OVER(ORDER BY sum(number)-min(number) DESC) AS rn
FROM section_data
GROUP BY section)
,
final_sol as (
select s.*, rn,
dense_rank() over (partition by rn order by a.max_no desc) as den_rnk
from agg_data as a
inner join section_data as s on a.section = s.section and  s.number != a.min_no
where rn <=2
)

select section, number from final_sol
where rn <= 2 and den_rnk = 1```