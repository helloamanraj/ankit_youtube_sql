question: https://www.youtube.com/watch?v=KWOZ9VoVFac&list=PLBTZqjSKn0IfuIqbMIqzS-waofsPHMS0E&index=14

```sql

create table Ameriprise_LLC
(
teamID varchar(2),
memberID varchar(10),
Criteria1 varchar(1),
Criteria2 varchar(1)
);
insert into Ameriprise_LLC values 
('T1','T1_mbr1','Y','Y'),
('T1','T1_mbr2','Y','Y'),
('T1','T1_mbr3','Y','Y'),
('T1','T1_mbr4','Y','Y'),
('T1','T1_mbr5','Y','N'),
('T2','T2_mbr1','Y','Y'),
('T2','T2_mbr2','Y','N'),
('T2','T2_mbr3','N','Y'),
('T2','T2_mbr4','N','N'),
('T2','T2_mbr5','N','N'),
('T3','T3_mbr1','Y','Y'),
('T3','T3_mbr2','Y','Y'),
('T3','T3_mbr3','N','Y'),
('T3','T3_mbr4','N','Y'),
('T3','T3_mbr5','Y','N');

```

Solution: 

```sql
with cte as (
select teamID,
sum(case when Criteria1 ='Y' and Criteria2 ='Y' then 1 else 0 end ) as total
from Ameriprise_LLC
group by teamID
having total > 1
)

select c.teamID, memberID, Criteria1, Criteria2 , 'Y' as output from cte as c
left join Ameriprise_LLC as a on c.teamID = a.teamID
where Criteria1 = 'Y' and Criteria2 = 'Y'
```