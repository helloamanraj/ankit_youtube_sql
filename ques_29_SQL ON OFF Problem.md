Question 29:
https://www.youtube.com/watch?v=XQ80MgsTka0&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=29
SQL ON OFF Problem:

```sql
create table event_status
(
event_time varchar(10),
status varchar(10)
)

insert into event_status
values
('10:01','on'),('10:02','on'),('10:03','on'),('10:04','off'),('10:07','on'),('10:08','on'),('10:09','off')
,('10:11','on'),('10:12','off');
```

Solution:
```sql
with cte as(
select *, lag(status,1) over (order by event_time) as prev_status from event_status
)
,cte2 as (
select event_time, status , sum(case when status = 'on' and prev_status = 'off' then 1 else 0 end) over(order by event_time) as grp from cte
)

select min(event_time) as in_time , max(event_time) as out_time , FLOOR(TIME_TO_SEC(TIMEDIFF(MAX(event_time), MIN(event_time))) / 60) as duration from cte2
group by grp
```