Question: https://www.youtube.com/watch?v=J9wwR4huimI&list=PLBTZqjSKn0IfuIqbMIqzS-waofsPHMS0E&index=20

```sql

create table source(id int, name varchar(5))

create table target(id int, name varchar(5))

insert into source values(1,'A'),(2,'B'),(3,'C'),(4,'D')

insert into target values(1,'A'),(2,'B'),(4,'X'),(5,'F');
```


Solution1: 
```sql
with cte as (
select *, concat(id,name) as ct, 'ent_sou' as col_table from source
union all
select *, concat(id,name) as ct, 'ent_target' as col_table from target
)
select distinct id,   
case when (count(col_table) over (partition by id order by id)) =1  then col_table
else 'mismatch' END AS comment
from cte
where ct not in (select concat(id,name) from target)
 or ct not in  (select concat(id,name) from source)
```

Solution2: 
```sql

with cte as (

select s.*,t.id as t_id, t.name as t_name, 'new_in_source' as tbl from source as s 
left join target as t on s.id = t.id

union all

select s.*, t.id as t_id, t.name as t_name ,'new_in_target' from source as s 
right join target as t on s.id = t.id
)
 

select distinct com_id, 
case 
	when name is null then 'new in source' 
	when t_name is null then 'new in target' 
	else 'mismatch' 
end as comt 
from (select coalesce(id,t_id) as com_id, name, t_name from cte
 where t_id is null or id is null or (id = t_id and name!= t_name
 )) x

 ```




