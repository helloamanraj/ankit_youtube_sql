Question link: https://www.youtube.com/watch?v=aGKzhAkkOP8&t=518s


```sql
CREATE TABLE friends (
    user_id INT,
    friend_id INT
);

-- Insert data into friends table
INSERT INTO friends VALUES
(1, 2),
(1, 3),
(1, 4),
(2, 1),
(3, 1),
(3, 4),
(4, 1),
(4, 3);

-- Create likes table
CREATE TABLE likes (
    user_id INT,
    page_id CHAR(1)
);

-- Insert data into likes table
INSERT INTO likes VALUES
(1, 'A'),
(1, 'B'),
(1, 'C'),
(2, 'A'),
(3, 'B'),
(3, 'C'),
(4, 'B');
```


Solution1: 
```sql
with cte as ( 
select f.user_id as user_id1, friend_id,l.*  from friends as f
inner join likes as l on f.friend_id = l.user_id
)
,cte2 as (
select user_id1 , c.friend_id, l.page_id, 
c.page_id as pge_id  from cte as c
left join likes as l on c.user_id1 = l.user_id 
and c.page_id = l.page_id
where l.page_id is null
)

select distinct user_id1, friend_id, group_concat(pge_id) as page_id from cte2
group by user_id1, friend_id
```

Solution 2: 

```sql

with cte as
(
select distinct f.user_id, l.page_id from friends f
join likes l on f.friend_id = l.user_id
)

select user_id, page_id from cte
where (user_id, page_id) not in (select user_id, page_id from likes)
```

Solution3: 

```sql
with user_cte as ( 
select distinct f.user_id ,l.page_id as page_id from friends as f
inner join likes as l on f.user_id = l.user_id
)
,

friend_cte as ( 
select distinct f.user_id , f.friend_id, l.page_id as page_id_1   from friends as f
inner join likes as l on f.friend_id = l.user_id
)

select * from friend_cte as f
left join user_cte as u on f.user_id = u.user_id and f.page_id_1 = u.page_id
where page_id is null
```