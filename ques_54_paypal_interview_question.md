Question 53:
https://www.youtube.com/watch?v=UrIrBraLvZU&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=54


Paypal interview sql:

```sql
create table emp(
emp_id int,
emp_name varchar(20),
department_id int,
salary int,
manager_id int,
emp_age int);

insert into emp
values
(1, 'Ankit', 100,10000, 4, 39);
insert into emp
values (2, 'Mohit', 100, 15000, 5, 48);
insert into emp
values (3, 'Vikas', 100, 10000,4,37);
insert into emp
values (4, 'Rohit', 100, 5000, 2, 16);
insert into emp
values (5, 'Mudit', 200, 12000, 6,55);
insert into emp
values (6, 'Agam', 200, 12000,2, 14);
insert into emp
values (7, 'Sanjay', 200, 9000, 2,13);
insert into emp
values (8, 'Ashish', 200,5000,2,12);
insert into emp
values (9, 'Mukesh',300,6000,6,51);
insert into emp
values (10, 'Rakesh',300,7000,6,50);
```

Solution:

```sql
WITH CTE as(
SELECT *,round(avg(salary)OVER(PARTITION BY department_id),0)AS avg_dept_salary,
round(count(emp_id)OVER(PARTITION BY department_id),0) AS emp_dept_count,
round(avg(salary)OVER(),0) AS total_avg_salary,
count(emp_id)OVER() AS emp_overall_count
FROM emp),
cte_2 AS(
SELECT *,((total_avg_salary*emp_overall_count)-(emp_dept_count*avg_dept_salary))/(emp_overall_count-emp_dept_count) AS avg_desired
FROM CTE)
SELECT * FROM cte_2
WHERE avg_dept_salary<avg_desired```