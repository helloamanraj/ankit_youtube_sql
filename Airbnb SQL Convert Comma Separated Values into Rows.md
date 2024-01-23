question link: https://www.youtube.com/watch?v=PE5MZW1CxOI&list=PLBTZqjSKn0IfuIqbMIqzS-waofsPHMS0E&index=4

```sql

create table airbnb_searches 
(
user_id int,
date_searched date,
filter_room_types varchar(200)
);

insert into airbnb_searches values
(1,'2022-01-01','entire home,private room')
,(2,'2022-01-02','entire home,shared room')
,(3,'2022-01-02','private room,shared room')
,(4,'2022-01-03','private room')
;

```

Solution 1: 
```sql

with recursive split as (
select 
user_id, date_searched
,1 as n, 
substring_index(filter_room_types, ',', 1) as room_type 
from airbnb_searches 

union all

select airbnb_searches.user_id, airbnb_searches.date_searched, 
n+1, 
substring_index(substring_index(filter_room_types, ',', n+1),',','-1') as room_type
from split join airbnb_searches
on airbnb_searches.user_id = split.user_id 
where length(airbnb_searches.filter_room_types) > length(replace(airbnb_searches.filter_room_types, ',', '')) * n

)


select room_type, count(room_type) from split
group by room_type
order by count(room_type) desc

```