Question 08:
https://www.youtube.com/watch?v=IQ4n4n-Y9z8&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=8


Leetcode Hard problem 2| Tournament Winners

```sql create table players
(player_id int,
group_id int)

insert into players values (15,1);
insert into players values (25,1);
insert into players values (30,1);
insert into players values (45,1);
insert into players values (10,2);
insert into players values (35,2);
insert into players values (50,2);
insert into players values (20,3);
insert into players values (40,3);

create table matches
(
match_id int,
first_player int,
second_player int,
first_score int,
second_score int)

insert into matches values (1,15,45,3,0);
insert into matches values (2,30,25,1,2);
insert into matches values (3,30,15,2,0);
insert into matches values (4,40,20,5,2);
insert into matches values (5,35,50,1,1); 
```



Solution:

```sql 
with cte as (
 select first_Player as player_id , first_score as score from matches
 
 union all
 
 select second_player as player_id , second_score as score from matches
 )
 , cte2  as (
 select group_id, c.player_id , score, rank() over (partition by group_id order by score desc , player_id ) as rnk
 from cte as c
 inner join players as p on c.player_id  = p.player_id
)

select *
from cte2
where rnk = 1
```