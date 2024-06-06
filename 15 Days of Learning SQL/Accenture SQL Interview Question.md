Question: https://www.youtube.com/watch?v=WmY_0shtpdg

```sql
 
CREATE TABLE employees  (employee_id int,employee_name varchar(15), email_id varchar(15) );

INSERT INTO employees (employee_id,employee_name, email_id) VALUES ('101','Liam Alton', 'li.al@abc.com');
INSERT INTO employees (employee_id,employee_name, email_id) VALUES ('102','Josh Day', 'jo.da@abc.com');
INSERT INTO employees (employee_id,employee_name, email_id) VALUES ('103','Sean Mann', 'se.ma@abc.com'); 
INSERT INTO employees (employee_id,employee_name, email_id) VALUES ('104','Evan Blake', 'ev.bl@abc.com');
INSERT INTO employees (employee_id,employee_name, email_id) VALUES ('105','Toby Scott', 'jo.da@abc.com');
INSERT INTO employees (employee_id,employee_name, email_id) VALUES ('106','Anjali Chouhan', 'JO.DA@ABC.COM');
INSERT INTO employees (employee_id,employee_name, email_id) VALUES ('107','Ankit Bansal', 'AN.BA@ABC.COM');

```

Solution:

```sql
with cte as (
select employee_id, employee_name,  email_id,count(*) over (partition by email_id) as cnt from employees
)
, cte2 as (
select employee_id, employee_name, lower(email_id) as lw, email_id from cte
where cnt > 1
)

select * from cte2 
where ascii(lw) = ascii(email_id)
```