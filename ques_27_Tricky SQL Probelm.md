Question 27:
https://www.youtube.com/watch?v=Ck1gQrlS5pQ&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=27

Solving 4 Tricky SQL Problems:


```sql
CREATE TABLE students(
  studentid INT,
  studentname VARCHAR(255),
  subject VARCHAR(255),
  marks INT,
  testid INT,
  testdate DATE
)


INSERT INTO students VALUES (2, 'Max Ruin', 'Subject1', 63, 1, '2022-01-02');
INSERT INTO students VALUES (3, 'Arnold', 'Subject1', 95, 1, '2022-01-02');
INSERT INTO students VALUES (4, 'Krish Star', 'Subject1', 61, 1, '2022-01-02');
INSERT INTO students VALUES (5, 'John Mike', 'Subject1', 91, 1, '2022-01-02');
INSERT INTO students VALUES (4, 'Krish Star', 'Subject2', 71, 1, '2022-01-02');
INSERT INTO students VALUES (3, 'Arnold', 'Subject2', 32, 1, '2022-01-02');
INSERT INTO students VALUES (5, 'John Mike', 'Subject2', 61, 2, '2022-11-02');
INSERT INTO students VALUES (1, 'John Deo', 'Subject2', 60, 1, '2022-01-02');
INSERT INTO students VALUES (2, 'Max Ruin', 'Subject2', 84, 1, '2022-01-02');
INSERT INTO students VALUES (2, 'Max Ruin', 'Subject3', 29, 3, '2022-01-03');
INSERT INTO students VALUES (5, 'John Mike', 'Subject3', 98, 2, '2022-11-02');
```

Solution 27.1: 

```sql
with cte as (
select *, round(avg(marks) over (partition by subject order by testdate),2) as avg_score from students
)

select * from cte
where avg_score < marks
```

Solution 27.2:
```sql
select 
round(100*count(distinct case when marks > 90 then studentid  else null end ) / count(distinct studentid),2) as percentage
from students 
```

Solution 27.3:

```sql

with cte as (

select * , rank() over (partition by subject order by marks) as rnk_asc,
rank() over (partition by subject order by marks desc)  as rnk_dsc, 
count(*) over (partition by subject ) as cnt
from students

)

SELECT subject,  MAX(CASE 
            WHEN cnt > 2 AND rnk_asc = 2 THEN marks 
            WHEN cnt <= 2 AND rnk_asc = 1 THEN marks 
            ELSE NULL 
        END) AS second_smallest_marks,
    MAX(CASE 
            WHEN cnt > 2 AND rnk_dsc = 2 THEN marks 
            WHEN cnt <= 2 AND rnk_dsc = 1 THEN marks 
            ELSE NULL 
        END) AS second_highest_marks
FROM cte 
GROUP BY subject;
```

Solution 27.4:

```sql
with cte as (
select *, lead(marks,1 ) over (partition by studentname order by subject) as next_marks from students 
order by studentid
)

select *, case when marks < next_marks then "Incre" 
when next_marks < marks then "Desc" else Null
 end as marks from cte
 
 ```