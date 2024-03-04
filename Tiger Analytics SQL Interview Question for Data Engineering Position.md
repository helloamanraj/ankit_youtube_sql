Question link: https://www.youtube.com/watch?v=02XLUeIVRSE&list=PLBTZqjSKn0IfuIqbMIqzS-waofsPHMS0E&index=15


```sql 
create table family 
(
person varchar(5),
type varchar(10),
age int
);

insert into family values ('A1','Adult',54)
,('A2','Adult',53),('A3','Adult',52),('A4','Adult',58),('A5','Adult',54),('C1','Child',20),('C2','Child',19),('C3','Child',22),('C4','Child',15);

```

Solution1: 

```sql
elect a.person, b.person from (
select person, type, left(person,1) as grp,
row_number() over(partition by left(person,1) order by person) as rw_nm
from family
where type = 'Adult'
) as a 
left join 
(select person, type, left(person,1) as grp,
row_number() over(partition by left(person,1) order by person) as rw_nm
from family
where type = 'Child'
) as b
on a.rw_nm = b.rw_nm
```

Solution2: 
```sql
with lft as (
select person, type, left(person,1) as grp,
row_number() over(partition by left(person,1) order by person) as rw_nm
from family as c1
where left(person,1) = 'A'
)
, rgh as (
select person, type, left(person,1) as grp,
row_number() over(partition by left(person,1) order by person) as rw_nm
from family as c1
where left(person,1) = 'C'
)

select l.person, r.person from lft as l left join rgh as r on
l.rw_nm = r.rw_nm
```