Question link: https://www.youtube.com/watch?v=QFZG-IHC8xk&t=2s

```sql

CREATE TABLE king (
    k_no INT PRIMARY KEY,
    king VARCHAR(50),
    house VARCHAR(50)
);

-- Create the 'battle' table
CREATE TABLE battle (
    battle_number INT PRIMARY KEY,
    name VARCHAR(100),
    attacker_king INT,
    defender_king INT,
    attacker_outcome INT,
    region VARCHAR(50),
    FOREIGN KEY (attacker_king) REFERENCES king(k_no),
    FOREIGN KEY (defender_king) REFERENCES king(k_no)
);

delete from king;
INSERT INTO king (k_no, king, house) VALUES
(1, 'Robb Stark', 'House Stark'),
(2, 'Joffrey Baratheon', 'House Lannister'),
(3, 'Stannis Baratheon', 'House Baratheon'),
(4, 'Balon Greyjoy', 'House Greyjoy'),
(5, 'Mace Tyrell', 'House Tyrell'),
(6, 'Doran Martell', 'House Martell');

delete from battle;
-- Insert data into the 'battle' table
INSERT INTO battle (battle_number, name, attacker_king, defender_king, attacker_outcome, region) VALUES
(1, 'Battle of Oxcross', 1, 2, 1, 'The North'),
(2, 'Battle of Blackwater', 3, 4, 0, 'The North'),
(3, 'Battle of the Fords', 1, 5, 1, 'The Reach'),
(4, 'Battle of the Green Fork', 2, 6, 0, 'The Reach'),
(5, 'Battle of the Ruby Ford', 1, 3, 1, 'The Riverlands'),
(6, 'Battle of the Golden Tooth', 2, 1, 0, 'The North'),
(7, 'Battle of Riverrun', 3, 4, 1, 'The Riverlands'),
(8, 'Battle of Riverrun', 1, 3, 0, 'The Riverlands');

```


Solution1:
```sql

with cte as (
select *, 
case when attacker_outcome = 0 then defender_king else attacker_king end as winner 
from battle
)

,cte2 as (select region, house, cnt ,dense_rank() over (partition by region order by cnt desc) as rnk from 
(select region, house, count(*)  as cnt
from cte as c
inner join king as k on c.winner = k.k_no
group by region , house 
) as x
)
select * from cte2
where rnk = 1
```
