Question: https://www.youtube.com/watch?v=EsGPj8Ojk2o&list=PLBTZqjSKn0IfuIqbMIqzS-waofsPHMS0E&index=17
```sql

create table people
(id int primary key not null,
 name varchar(20),
 gender char(2));

create table relations
(
    c_id int,
    p_id int,
    FOREIGN KEY (c_id) REFERENCES people(id),
    foreign key (p_id) references people(id)
);

insert into people (id, name, gender)
values
    (107,'Days','F'),(145,'Hawbaker','M'),(155,'Hansel','F'),(202,'Blackston','M'),(227,'Criss','F'),(278,'Keffer','M'),
    (305,'Canty','M'),(329,'Mozingo','M'),(425,'Nolf','M'),(534,'Waugh','M'),(586,'Tong','M'),(618,'Dimartino','M'),
    (747,'Beane','M'),
    (878,'Chatmon','F'),
    (904,'Hansard','F');

insert into relations(c_id, p_id)
values
    (145, 202),(145, 107),(278,305),(278,155),(329, 425),(329,227),(534,586),(534,878),(618,747),(618,904);
```    

solution 1:

```sql
with cte as (
select c_id, 
max(case when gender = 'M' then name end) as father,
max(case when gender = 'F' then name end) as Mother
from people as p
inner join relations as r on p.id = r.p_id 
group by c_id
)

select p.name as child_name ,father, mother from cte as c
join people as p on p.id = c.c_id
```

Solution 2:

```sql
with cte as (
select r.c_id as id, max(p.name) as mother_name, max(q.name) as father_name from relations as r
left join people as p on p.id = r.p_id and p.gender = 'F'
left join people as q on q.id = r.p_id and q.gender = 'M'
group by r.c_id
)

select name as child_name , mother_name , father_name from cte as c
inner join people as p on c.id = p.id
```