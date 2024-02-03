Question: https://www.youtube.com/watch?v=3sd5siyCAOo&list=PLBTZqjSKn0IfuIqbMIqzS-waofsPHMS0E&index=13


```sql
create table input (
id int,
formula varchar(10),
value int
)
insert into input values (1,'1+4',10),(2,'2+1',5),(3,'3-2',40),(4,'4-1',20)

```

Solution1:

```sql
with cte as (
SELECT *, left(formula,1) as lt, right(formula,1) rh
FROM input
)

select c.id, c.value as val_1, c.formula, i.id, i.value as val_2
, 
case when substring(c.formula,2,1) = '+' then c.value + i.value 
when substring(c.formula,2,1) = '-' then c.value - i.value else null end as new_value
from cte as c
inner join input as i on c.rh = i.id 
order by c.id 
```

Solution2: 

```sql
with cte as (

select *, right(formula, 1) as rt from input
)

select c.id, c.formula,c.value, rt, i.value as up_val,
(case when substring(c.formula,2,1) = '+' then  c.value + i.value 
when substring(c.formula,2,1) = '-' then  c.value - i.value end) as final_val
from cte as c
left join input as i on c.rt = i.id
```