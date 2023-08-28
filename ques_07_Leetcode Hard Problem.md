Question 07:
https://www.youtube.com/watch?v=EjzhMv0E_FE&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=7


Leetcode Hard Problem:

```sql
Create table  Trips (id int, client_id int, driver_id int, city_id int, status varchar(50), request_at varchar(50));

drop table users
Create table Users (users_id int, banned varchar(50), role varchar(50));

insert into Trips (id, client_id, driver_id, city_id, status, request_at) values ('1', '1', '10', '1', 'completed', '2013-10-01');
insert into Trips (id, client_id, driver_id, city_id, status, request_at) values ('2', '2', '11', '1', 'cancelled_by_driver', '2013-10-01');
insert into Trips (id, client_id, driver_id, city_id, status, request_at) values ('3', '3', '12', '6', 'completed', '2013-10-01');
insert into Trips (id, client_id, driver_id, city_id, status, request_at) values ('4', '4', '13', '6', 'cancelled_by_client', '2013-10-01');
insert into Trips (id, client_id, driver_id, city_id, status, request_at) values ('5', '1', '10', '1', 'completed', '2013-10-02');
insert into Trips (id, client_id, driver_id, city_id, status, request_at) values ('6', '2', '11', '6', 'completed', '2013-10-02');
insert into Trips (id, client_id, driver_id, city_id, status, request_at) values ('7', '3', '12', '6', 'completed', '2013-10-02');
insert into Trips (id, client_id, driver_id, city_id, status, request_at) values ('8', '2', '12', '12', 'completed', '2013-10-03');
insert into Trips (id, client_id, driver_id, city_id, status, request_at) values ('9', '3', '10', '12', 'completed', '2013-10-03');
insert into Trips (id, client_id, driver_id, city_id, status, request_at) values ('10', '4', '13', '12', 'cancelled_by_driver', '2013-10-03');

insert into Users (users_id, banned, role) values ('1', 'No', 'client');
insert into Users (users_id, banned, role) values ('2', 'Yes', 'client');
insert into Users (users_id, banned, role) values ('3', 'No', 'client');
insert into Users (users_id, banned, role) values ('4', 'No', 'client');
insert into Users (users_id, banned, role) values ('10', 'No', 'driver');
insert into Users (users_id, banned, role) values ('11', 'No', 'driver');
insert into Users (users_id, banned, role) values ('12', 'No', 'driver');
insert into Users (users_id, banned, role) values ('13', 'No', 'driver');
```

Solution:

```sql
with cte as (
select id, client_id, driver_id, status ,request_at , p.banned as cli_ban, u.banned as dri_band, p.role as r1, u.role as r2 from trips as t
left join users as u on u.users_id = t.client_id
left join users as p on p.users_id = t.driver_id
where p.banned = 'No' and u.banned= 'No'
and request_at between "2013-10-01" and "2013-10-03"
)
,
cte2 as (
select request_at,
sum(case when status in ("cancelled_by_client", "cancelled_by_driver") then 1 else 0 end) as cancelled_count,
count(*)  as total_count
from cte
group by request_at
order by request_at)

select *, round(100*(cancelled_count/ total_count),2) as cancellation_percentage from cte2```