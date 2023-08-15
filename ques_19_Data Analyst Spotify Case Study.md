Question 19:
https://www.youtube.com/watch?v=-YdAIMjHZrM&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=19

Data Analyst Spotify Case Study:

```sql

CREATE table activity
(
user_id varchar(20),
event_name varchar(20),
event_date date,
country varchar(20)
);
delete from activity;
insert into activity values (1,'app-installed','2022-01-01','India')
,(1,'app-purchase','2022-01-02','India')
,(2,'app-installed','2022-01-01','USA')
,(3,'app-installed','2022-01-01','USA')
,(3,'app-purchase','2022-01-03','USA')
,(4,'app-installed','2022-01-03','India')
,(4,'app-purchase','2022-01-03','India')
,(5,'app-installed','2022-01-03','SL')
,(5,'app-purchase','2022-01-03','SL')
,(6,'app-installed','2022-01-04','Pakistan')
,(6,'app-purchase','2022-01-04','Pakistan');






```


Solution:
19.1
```sql

select event_date , count(user_id) as cnt_by_user from activity 
group by event_date

```

19.2

```sql

select week(event_date, 0), count(distinct user_id) from activity
group by week(event_date)


```

19.3

```sql


 with cte as (
 select user_id, event_name,
 lead(event_name,1) over (partition by user_id )  as next_event,
 event_date, lead(event_date,1) over (partition by user_id ) as next_date
 from activity
)

select event_date,
sum(case when event_date = next_date and event_name != next_event then 1 else 0 end) as no_of_user_same_day_purchase  from cte
group by event_date




```

Another solution of 19.3

```sql

select event_date, count(new_user) no_of_user_same_day_purchase from
(select user_id, event_date, case when count(distinct event_name)=2 then user_id else null end as new_user from activity
group  by user_id , event_date)x
group by event_date



```

19.4

```sql

with cte as (
select *, count(country) as cnt ,
case when country= 'India' then country 
when country = 'USA' then country 
else "others" end as new_country
from activity 
where event_name = 'app-purchase'
group by new_country
)

select new_country, 100*cnt/ (select sum(cnt) from cte ) as percentage_users
from cte
group by new_country



```

19.5

```sql


with cte as(
select *, lag(event_name,1) over (partition by user_id order by event_date) as prev_event_name,
lag(event_date,1) over (partition by user_id order by event_date) as prev_event_date
from activity)

select event_date,
count(case when event_name='app-purchase' and prev_event_name='app-installed' and datediff(event_date,prev_event_date)=1 then 1 else null end) 
as user_cnt 
from cte
group by 1
order by 1;




```