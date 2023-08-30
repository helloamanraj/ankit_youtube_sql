Question 43:
https://www.youtube.com/watch?v=8glk10JlvKE&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=43





Amazon Interview Question:
```sql

create table emp(
emp_id int,
emp_name varchar(20),
department_id int,
salary int,
manager_id int,
emp_age int)

insert into emp
values
(1, 'Ankit', 100,10000, 4, 39);
insert into emp
values (2, 'Mohit', 100, 15000, 5, 48);
insert into emp
values (3, 'Vikas', 100, 12000,4,37);
insert into emp
values (4, 'Rohit', 100, 14000, 2, 16);
insert into emp
values (5, 'Mudit', 200, 20000, 6,55);
insert into emp
values (6, 'Agam', 200, 12000,2, 14);
insert into emp
values (7, 'Sanjay', 200, 9000, 2,13);
insert into emp
values (8, 'Ashish', 200,5000,2,12);
insert into emp
values (9, 'Mukesh',300,6000,6,51);
insert into emp
values (10, 'Rakesh',500,7000,6,50);```



Solution:
```sql
select a.emp_id, a.emp_name as emp,b.emp_id as manager_id, b.emp_name as manager_name , c.emp_id as sm_id,c.emp_name as sm_name
from emp as a
left join emp as b on a.manager_id = b.emp_id
left join emp as c on b.manager_id = c.emp_id
```