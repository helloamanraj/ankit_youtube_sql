Question 04:
https://www.youtube.com/watch?v=qyAgWL066Vo&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=1


Complex SQL Query 1:


```sql
create table icc_world_cup
(
Team_1 Varchar(20),
Team_2 Varchar(20),
Winner Varchar(20)
);
INSERT INTO icc_world_cup values('India','SL','India');
INSERT INTO icc_world_cup values('SL','Aus','Aus');
INSERT INTO icc_world_cup values('SA','Eng','Eng');
INSERT INTO icc_world_cup values('Eng','NZ','NZ');
INSERT INTO icc_world_cup values('Aus','India','India');
```
Solution:
```sql
with cte as
(select team_1,case when team_1 = winner then 1 else 0 end win_flag
from icc_world_cup
union all
select team_2, case when team_2 = winner then 1 else 0 end win_flag
from icc_world_cup)

select a.*, (a.number_of_match_played - number_of_win) number_of_loss from
(select team_1 team, count(*) number_of_match_played, sum(win_flag) number_of_win from cte
group by team_1)a
```