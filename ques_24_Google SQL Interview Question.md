Question 24:
https://www.youtube.com/watch?v=35gjU7pChQk&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=24

Google SQL Interview Question:


```sql

create table company_users 
(
company_id int,
user_id int,
language varchar(20)
);

insert into company_users values (1,1,'English')
,(1,1,'German')
,(1,2,'English')
,(1,3,'German')
,(1,3,'English')
,(1,4,'English')
,(2,5,'English')
,(2,5,'German')
,(2,5,'Spanish')
,(2,6,'German')
,(2,6,'Spanish')
,(2,7,'English');










```

Solution:

```sql


with cte as (
select company_id, count(user_id) from company_users
where language in  ("English", "German")
group by company_id, user_id
having count(user_id) >= 2
)
select company_id, count(company_id) as cnt_of_emp from cte
group by company_id 
having count(company_id) >= 2






```