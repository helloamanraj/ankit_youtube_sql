Question link: https://www.youtube.com/watch?v=Sq1kM3jVU68&t=97s

```sql
CREATE TABLE seats (
    id INT,
    student VARCHAR(10)
);

INSERT INTO seats VALUES 
(1, 'Amit'),
(2, 'Deepa'),
(3, 'Rohit'),
(4, 'Anjali'),
(5, 'Neha'),
(6, 'Sanjay'),
(7, 'Priya');
```

Solution:
```sql
select *,
case when id % 2 = 0 then lag(student,1, student) over (order by id) 
else lead(student,1,student) over (order by id) end as up_student
from seats
```

```sql
select id, student, 
case when id % 2 != 0 and id = (select max(id) from seats) then id
when id % 2 = 0 then id - 1 else id+ 1 end as new_id from seats
```

