Question 26:
https://www.youtube.com/watch?v=FZm7NgybHWA&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=26


Amazon Interview Question for BIE position:


```sql
 CREATE TABLE subscriber (
 sms_date date ,
 sender varchar(20) ,
 receiver varchar(20) ,
 sms_no int
);
-- insert some values
INSERT INTO subscriber VALUES ('2020-4-1', 'Avinash', 'Vibhor',10);
INSERT INTO subscriber VALUES ('2020-4-1', 'Vibhor', 'Avinash',20);
INSERT INTO subscriber VALUES ('2020-4-1', 'Avinash', 'Pawan',30);
INSERT INTO subscriber VALUES ('2020-4-1', 'Pawan', 'Avinash',20);
INSERT INTO subscriber VALUES ('2020-4-1', 'Vibhor', 'Pawan',5);
INSERT INTO subscriber VALUES ('2020-4-1', 'Pawan', 'Vibhor',8);
INSERT INTO subscriber VALUES ('2020-4-1', 'Vibhor', 'Deepak',50);
```

Solution: 

```sql
with cte as (
select sms_date, sender, receiver,
case when sender > receiver then concat(sender,' ', receiver)  else concat(receiver,' ',sender) end as couple,
sms_no
from subscriber
)

select sms_date, couple,max(sender) as sender, max(receiver) as receiver,  sum(sms_no) from cte 
group by 1,2```