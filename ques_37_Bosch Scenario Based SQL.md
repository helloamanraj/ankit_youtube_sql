Question 37:
https://www.youtube.com/watch?v=pk8BKFysjP8&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=37

Bosch Scenario Based SQL

```sql
create table call_details  (
call_type varchar(10),
call_number varchar(12),
call_duration int
)


insert into call_details
values ('OUT','181868',13),('OUT','2159010',8)
,('OUT','2159010',178),('SMS','4153810',1),('OUT','2159010',152),('OUT','9140152',18),('SMS','4162672',1)
,('SMS','9168204',1),('OUT','9168204',576),('INC','2159010',5),('INC','2159010',4),('SMS','2159010',1)
,('SMS','4535614',1),('OUT','181868',20),('INC','181868',54),('INC','218748',20),('INC','2159010',9)
,('INC','197432',66),('SMS','2159010',1),('SMS','4535614',1);
```
Solution: 
```sql

with cte as (

select *, sum(call_duration) as call_In_out_duration from call_details
where call_type in ('out', 'inc')
group by call_type, call_number
order by call_number
)
, cte2 as (
select *, count(call_type) over (partition by call_number) as call_num_count  from cte
)

, cte3 as (
select * from cte2
where call_num_count > 1
)


select c3.call_number from cte3 as c3
inner join (
select call_number , max(call_In_out_duration) as max_in_out from cte3
where call_type = 'INC'
group by call_number
) as c4
on c3.call_number = c4.call_number
where call_type = 'out' and c3.call_in_out_duration > c4.max_in_out
```