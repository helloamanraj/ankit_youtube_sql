Question 17:
https://www.youtube.com/watch?v=RljzVfz8vjk&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=17

Leetcode Hard SQL Problem - 6:


```sql

create table UserActivity
(
username      varchar(20) ,
activity      varchar(20),
startDate     Date   ,
endDate      Date
);

insert into UserActivity values 
('Alice','Travel','2020-02-12','2020-02-20')
,('Alice','Dancing','2020-02-21','2020-02-23')
,('Alice','Travel','2020-02-24','2020-02-28')
,('Bob','Travel','2020-02-11','2020-02-18');




```
Solution:

```sql


with cte as (
select *, rank() over (partition by username order by endDate desc) as rnk,
count(username) over (partition by username) as grp
from useractivity
)

SELECT username, activity ,startdate, enddate from cte  
where grp =1 or rnk =2 







```