question link: https://www.youtube.com/watch?v=FZ0GCcnIIWA&list=PLBTZqjSKn0IfuIqbMIqzS-waofsPHMS0E&index=2

```sql
create table tickets
(
ticket_id varchar(10),
create_date date,
resolved_date date
);

insert into tickets values
(1,'2022-08-01','2022-08-03')
,(2,'2022-08-01','2022-08-12')
,(3,'2022-08-01','2022-08-16');
create table holidays
(
holiday_date date
,reason varchar(100)
);

insert into holidays values
('2022-08-11','Rakhi'),('2022-08-15','Independence day');

```
Solution: 

```sql
with cte as (
select *, datediff(resolved_date, create_date) - 
2* (week(resolved_date) - week(create_date)) as days
from tickets 
left join holidays on holiday_date between create_date and resolved_date
)

select ticket_id, create_date , resolved_date , days - count(holiday_date) as Business_days  from cte  
group by ticket_id, create_date , resolved_date 

```