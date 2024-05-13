Question: https://www.youtube.com/watch?v=JHUlQZrviCI

```sql
CREATE TABLE city_distance
(
    distance INT,
    source VARCHAR(512),
    destination VARCHAR(512)
);


INSERT INTO city_distance(distance, source, destination) VALUES ('100', 'New Delhi', 'Panipat');
INSERT INTO city_distance(distance, source, destination) VALUES ('200', 'Ambala', 'New Delhi');
INSERT INTO city_distance(distance, source, destination) VALUES ('150', 'Bangalore', 'Mysore');
INSERT INTO city_distance(distance, source, destination) VALUES ('150', 'Mysore', 'Bangalore');
INSERT INTO city_distance(distance, source, destination) VALUES ('250', 'Mumbai', 'Pune');
INSERT INTO city_distance(distance, source, destination) VALUES ('250', 'Pune', 'Mumbai');
INSERT INTO city_distance(distance, source, destination) VALUES ('2500', 'Chennai', 'Bhopal');
INSERT INTO city_distance(distance, source, destination) VALUES ('2500', 'Bhopal', 'Chennai');
INSERT INTO city_distance(distance, source, destination) VALUES ('60', 'Tirupati', 'Tirumala');
INSERT INTO city_distance(distance, source, destination) VALUES ('80', 'Tirumala', 'Tirupati');
```

Solution1:
```sql

with cte as (
Select *, row_number () over (order by distance,source, destination) as rw from city_distance
)
select distance, source, destination from (select distance, source, destination, rw , min(rw) over (partition by distance) as mini from cte) as x
where rw = mini
```

Solution2:

```sql
with cte as (
Select *, row_number () over (order by distance,source, destination) as rw from city_distance
)

select c1.distance, c1.source, c1.destination from cte as c1 
left join cte as c2 on c1.source = c2.destination and c1.destination = c2.source 
where c2.distance is null or c1.distance != c2.distance or c1.rw < c2.rw 

```

Solution3:

```sql
with cte as (
Select *, 
case when source < destination then source else destination end as city1,
case when source < destination then destination else source end as city2
from city_distance 
)
,cte2 as (
select *, count(*) over (partition by city1, city2, distance ) as cnt from cte )

select distance , source, destination from cte2 
where cnt = 1 or source < destination
```