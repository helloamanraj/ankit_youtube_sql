Question 40:
https://www.youtube.com/watch?v=6CH7IU4yB5I&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=40



Leetcode-1412 Hard SQL Problem:

```sql
create table exams
(
exam_id int,
student_id int,
score int);

insert into exams values
(10,1,70),(10,2,80),(10,3,90),(20,1,80),(30,1,70),(30,3,80),(30,4,90),(40,1,60)
,(40,2,70),(40,4,80);

drop table students
create table students
(
student_id int,
student_name varchar(20)
);
insert into students values
(1,'Daniel'),(2,'Jade'),(3,'Stella'),(4,'Jonathan'),(5,'Will');```


Solution:

```sql
with cte as (
 SELECT *, MIN(score) OVER (PARTITION BY exam_id) AS min_score,
    max(score) over (partition by exam_id) as max_score
    FROM exams
)

SELECT distinct (student_id)
FROM exams
WHERE student_id NOT IN (
  SELECT student_id
  FROM cte
  WHERE score = min_score or score = max_score
)
```