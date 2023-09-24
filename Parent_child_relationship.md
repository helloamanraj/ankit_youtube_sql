question link: https://www.linkedin.com/feed/update/urn:li:activity:7110464107660644354/
question: Write a query that prints the names of a child and his parents in individual columns respectively in order of the name of the child as shown in the picture.

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
(107,'Days','F'),
(145,'Hawbaker','M'),
(155,'Hansel','F'),
(202,'Blackston','M'),
(227,'Criss','F'),
(278,'Keffer','M'),
(305,'Canty','M'),
(329,'Mozingo','M'),
(425,'Nolf','M'),
(534,'Waugh','M'),
(586,'Tong','M'),
(618,'Dimartino','M'),
(747,'Beane','M'),
(878,'Chatmon','F'),
(904,'Hansard','F');

insert into relations(c_id, p_id)
values
(145, 202),
(145, 107),
(278,305),
(278,155),
(329, 425),
(329,227),
(534,586),
(534,878),
(618,747),
(618,904);
```

Solution 1:
```sql
with cte as (
select c_id, p.name , p_id , t.name as parent, t.gender as par_gender from relations as r
left join people as p on r.c_id = p.id 
left join people as t on r.p_id = t.id
)

select name, 
max(case when par_gender = "M" then parent end) as father,
max(case when par_gender = "F" then parent end) as mother
from cte
group by name
```

Solution 2:
```sql
SELECT
    c.name AS child,
    father.name AS fathername,
    mother.name AS mothername
FROM
    (SELECT
            r.c_id,
            MAX(CASE WHEN p.gender = 'M' THEN p.name END) AS father_name,
            MAX(CASE WHEN p.gender = 'F' THEN p.name END) AS mother_name
        FROM
            relations AS r
        LEFT JOIN
            people AS p ON r.p_id = p.id
        GROUP BY
            r.c_id
    ) AS parent_data
LEFT JOIN
    people AS c ON parent_data.c_id = c.id
LEFT JOIN
    people AS father ON father.name = parent_data.father_name
LEFT JOIN
    people AS mother ON mother.name = parent_data.mother_name;
```