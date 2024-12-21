Question 18:
https://www.youtube.com/watch?v=51ryMCf-fvU&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=18

Scenario Based SQL Question:

```sql

create table HoursWorked 
(
emp_name varchar(20),
work_date date,
bill_hrs int
)
insert into HoursWorked values
('Sachin', '1990-07-01', 3),
('Sachin', '1990-08-01', 5),
('Sehwag', '1990-07-01', 2),
('Sachin', '1991-07-01', 4);


create table billings 
(
emp_name varchar(10),
bill_date date,
bill_rate int
)
delete from billings;
insert into billings values
('Sachin', '1990-01-01', 25),
('Sehwag', '1989-01-01', 15),
('Dhoni', '1989-01-01', 20),
('Sachin', '1991-02-05', 30);
```
Solution: 

```sql
with bill AS
(select emp_name,bill_rate,bill_date, lead(bill_date,1,'9999-12-31') over(partition by emp_name order by bill_date) as enddate
from billings
)
select a.emp_name,sum(bill_hrs*bill_rate) as total_house_costs from HoursWorked a join bill b on a.emp_name=b.emp_name and a.work_date between b.bill_date and b.enddate
group by a.emp_name
```


