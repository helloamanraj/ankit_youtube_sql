Question 06:
https://www.youtube.com/watch?v=SfzbR69LquU&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=6




Complex SQL 6:

Dataset: https://drive.google.com/drive/folders/1Dc81McsB4lp1JUIwssDmmOaw6Z7rBK8T

Solution: 
```sql
with cte as (
select f.personid, friendid, count(friendid) as cntoffriend
,sum(score) as ttl_score from friend as f
left join person as p on p.personID = f.FriendID
group by f.personid
having ttl_score > 100
)

select c.* , p.name from cte as c
inner join person as p on p.personid = c.personid
```

```sql

with cte as ( 
SELECT  f.*
FROM superstore.person AS p
LEFT JOIN superstore.friend AS f ON p.PersonID = f.PersonID
)
, cte2 as (select c.personid as personid, score from superstore.person as sp
join cte as c on sp.personid = c.friendid
)

select personid, sum(score) as total_friends_score, count(*) as cnt from cte2 
group by personid
having sum(score) >= 100
```