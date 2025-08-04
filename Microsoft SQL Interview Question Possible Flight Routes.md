Question:
https://www.youtube.com/watch?v=IFGEpvNSfLQ


```sql
drop table airports
CREATE TABLE airports (
 port_code VARCHAR(10) PRIMARY KEY,
 city_name VARCHAR(100)
);

drop table flights
CREATE TABLE flights (
 flight_id varchar (10),
 start_port VARCHAR(10),
 end_port VARCHAR(10),
 start_time datetime,
 end_time datetime
);


INSERT INTO airports (port_code, city_name) VALUES
('JFK', 'New York'),
('LGA', 'New York'),
('EWR', 'New York'),
('LAX', 'Los Angeles'),
('ORD', 'Chicago'),
('SFO', 'San Francisco'),
('HND', 'Tokyo'),
('NRT', 'Tokyo'),
('KIX', 'Osaka');

INSERT INTO flights VALUES
(1, 'JFK', 'HND', '2025-06-15 06:00', '2025-06-15 18:00'),
(2, 'JFK', 'LAX', '2025-06-15 07:00', '2025-06-15 10:00'),
(3, 'LAX', 'NRT', '2025-06-15 10:00', '2025-06-15 22:00'),
(4, 'JFK', 'LAX', '2025-06-15 08:00', '2025-06-15 11:00'),
(5, 'LAX', 'KIX', '2025-06-15 11:30', '2025-06-15 22:00'),
(6, 'LGA', 'ORD', '2025-06-15 09:00', '2025-06-15 12:00'),
(7, 'ORD', 'HND', '2025-06-15 11:30', '2025-06-15 23:30'),
(8, 'EWR', 'SFO', '2025-06-15 09:00', '2025-06-15 12:00'),
(9, 'LAX', 'HND', '2025-06-15 13:00', '2025-06-15 23:00'),
(10, 'KIX', 'NRT', '2025-06-15 08:00', '2025-06-15 10:00');
```

Answer:

```sql
with cte as (
SELECT 
a.start_port as initial_port, c.city_name as initial_city_name,a.start_time as initial_port_time, a.flight_id as start_flight,
a.end_port as connecting_end_port,d.city_name as connecting_end_city_name,  a.end_time as connecting_end_time, 
b.end_port,e.city_name as end_city_name,  b.end_time, b.flight_id as end_flight

FROM flights a LEFT JOIN flights b ON a.end_port =b.start_port AND a.end_time <= b.start_time and a.flight_id < b.flight_id
LEFT JOIN airports c ON c.port_code = a.start_port
LEFT JOIN airports d ON d.port_code = a.end_port
LEFT JOIN airports e ON e.port_code = b.end_port
)
,cte2 as (
SELECT *,
case when end_port is null then timestampdiff(minute, initial_port_time, connecting_end_time)
ELSE timestampdiff(minute, initial_port_time, end_time) end as "overall_time_taken_minutes"
FROM foundation
WHERE initial_city_name = "new york" AND (connecting_end_city_name= "tokyo" or end_city_name = "tokyo"))


SELECT *
, dense_rank()over(order by overall_time_taken_minutes ASC) as ranker
FROM final_data
```
