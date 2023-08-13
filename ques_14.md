 Question 14:
https://www.youtube.com/watch?v=i_ljK9gmstY&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=14

 Amazon Prime Subscription Rate SQL Logic:

```sql


drop table users
create table users
(
user_id integer,
name varchar(20),
join_date date
);

INSERT INTO users
VALUES
  (1, 'Jon', DATE '2020-02-14'),
  (2, 'Jane', DATE '2020-02-14'),
  (3, 'Jill', DATE '2020-02-15'),
  (4, 'Josh', DATE '2020-02-15'),
  (5, 'Jean', DATE '2020-02-16'),
  (6, 'Justin', DATE '2020-02-17'),
  (7, 'Jeremy', DATE '2020-02-18');

create table events
(
user_id integer,
type varchar(10),
access_date date
);

INSERT INTO events
VALUES
  (1, 'Pay', DATE '2020-03-01'),
  (2, 'Music', DATE '2020-03-02'),
  (2, 'P', DATE '2020-03-12'),
  (3, 'Music', DATE '2020-03-15'),
  (4, 'Music', DATE '2020-03-15'),
  (1, 'P', DATE '2020-03-16'),
  (3, 'P', DATE '2020-03-22');


```
   
   Solution:

```sql 


SELECT round(100*(count(distinct u.user_id) / (select count(distinct(user_id)) from events where type = 'Music')),2) as perc
FROM users AS u
LEFT JOIN events AS e ON u.user_id = e.user_id AND DATEDIFF(e.access_date, u.join_date) < 30 and e.TYPE = 'P'
where u.user_id in (select e.user_id from events where type = 'Music')

```
