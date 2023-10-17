question link: https://www.youtube.com/watch?v=t7mkWPga8lI&t=620s

```sql
create table icc_world_cup
(
match_no int,
team_1 Varchar(20),
team_2 Varchar(20),
winner Varchar(20)
);
INSERT INTO icc_world_cup values(1,'ENG','NZ','NZ');
INSERT INTO icc_world_cup values(2,'PAK','NED','PAK');
INSERT INTO icc_world_cup values(3,'AFG','BAN','BAN');
INSERT INTO icc_world_cup values(4,'SA','SL','SA');
INSERT INTO icc_world_cup values(5,'AUS','IND','IND');
INSERT INTO icc_world_cup values(6,'NZ','NED','NZ');
INSERT INTO icc_world_cup values(7,'ENG','BAN','ENG');
INSERT INTO icc_world_cup values(8,'SL','PAK','PAK');
INSERT INTO icc_world_cup values(9,'AFG','IND','IND');
INSERT INTO icc_world_cup values(10,'SA','AUS','SA');
INSERT INTO icc_world_cup values(11,'BAN','NZ','NZ');
INSERT INTO icc_world_cup values(12,'PAK','IND','IND');
INSERT INTO icc_world_cup values(12,'SA','IND','DRAW');
```
Solution 1 Aam Solution:

```sql

with cte as (
select team, sum(matches) as match_played from (
select team_1 as team, count(*) as matches from icc_world_cup
group by team_1
union all
select team_2 as team, count(*) as matches from icc_world_cup
group by team_2) as x
group by team
)
, cte2 as (
select winner, count(*) as w from icc_world_cup
where winner in (select distinct team from cte )
group  by winner
)

select cte.*, coalesce(w,0) wins, coalesce((match_played - w),0) as losses, coalesce(w,0) * 2 as points from cte 
left join cte2 on cte.team = cte2.winner
```

Solution 2 Mentos Solution:
```sql
select team, matches_played, w_flag as wins, matches_played - w_flag - d_flag as Losses, w_flag * 2 as points from
(
select team_1 as team, count(*) as matches_played,
sum(case when team_1 = winner then 1 else 0 end) as w_flag,
sum(case when winner = 'DRAW' then 1 else 0 end) as d_flag
from icc_world_cup
group by team_1

union all 

select team_2, count(*),
sum(case when team_1 = winner then 1 else 0 end) as w_flag,
sum(case when winner = 'DRAW' then 1 else 0 end) as d_flag
from icc_world_cup
group by team_2
)
as x
```