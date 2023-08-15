Question 22:
https://www.youtube.com/watch?v=aHShJ0hQtFc&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=22


Group By and Having Clause in SQL:


```sql

create table exams (student_id int, subject varchar(20), marks int);

insert into exams values 
(1,'Chemistry',91),(1,'Physics',91)
,(2,'Chemistry',80),(2,'Physics',90)
,(3,'Chemistry',80)
,(4,'Chemistry',71),(4,'Physics',54);



```


Solution:


```sql

with cte as (
select *,
rank() over (partition by student_id order by marks) as rnk
from exams)

select student_id from cte
where subject in ('chemistry', 'physics') and rnk = 1
group by student_id
having count(distinct subject) =2 and count(distinct rnk)=1



```