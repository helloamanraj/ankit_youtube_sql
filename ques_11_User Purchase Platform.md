Question 11:
https://www.youtube.com/watch?v=4MLVfsQEGl0&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=11


User Purchase Platform Leetcode Hard Problem:

```sql
create table spending 
(
user_id int,
spend_date date,
platform varchar(10),
amount int
);

insert into spending values(1,'2019-07-01','mobile',100),(1,'2019-07-01','desktop',100),(2,'2019-07-01','mobile',100)
,(2,'2019-07-02','mobile',100),(3,'2019-07-01','desktop',100),(3,'2019-07-02','desktop',100);

```

Solution 1:

```sql

with cte1 as (
		select user_id, spend_date, group_concat(platform separator ',') platform, sum(amount) amount
		from spending
		group by 1,2)
select spend_date, if(platform = 'mobile,desktop','both',platform) platform, sum(amount) total_amount, count(user_id) as total_users
from cte1 group by 1,2
union
select distinct spend_date, 'both' platform, 0 total_amount, 0 total_users
from spending where spend_date not in (select spend_date from cte1 where platform = 'mobile,desktop')
```

Solution 2:
```sql
with cte as (
(select spend_date,  max(platform) as platform, sum(amount) as amount from spending
group by spend_date, user_id
having count(distinct platform)= 1)
union all
(select spend_date,  "Both" as platform, sum(amount) amount from spending
group by spend_date, user_id
having count(distinct platform)= 2)
union 
(select distinct spend_date,  "Both" as platform , 0 as amount from spending)
)

select spend_date, platform, sum(amount) from cte
group by spend_date, platform
```