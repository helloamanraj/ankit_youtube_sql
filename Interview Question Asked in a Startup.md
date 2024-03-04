question link: https://www.youtube.com/watch?v=lxFQ0RgyEcA&list=PLBTZqjSKn0IfuIqbMIqzS-waofsPHMS0E&index=12

```sql
create table call_start_logs
(
phone_number varchar(10),
start_time datetime
);
insert into call_start_logs values
('PN1','2022-01-01 10:20:00'),('PN1','2022-01-01 16:25:00'),('PN2','2022-01-01 12:30:00')
,('PN3','2022-01-02 10:00:00'),('PN3','2022-01-02 12:30:00'),('PN3','2022-01-03 09:20:00')

create table call_end_logs
(
phone_number varchar(10),
end_time datetime
);
insert into call_end_logs values
('PN1','2022-01-01 10:45:00'),('PN1','2022-01-01 17:05:00'),('PN2','2022-01-01 12:55:00')
,('PN3','2022-01-02 10:20:00'),('PN3','2022-01-02 12:50:00'),('PN3','2022-01-03 09:40:00')
;
```

Solution: 

```sql
select srt.phone_number, start_time, end_time, timediff(end_time,start_time) as duration from 
(select phone_number, start_time , row_number() over (partition by phone_number) as rw_start from call_start_logs) as srt
inner join 
(select phone_number, end_time , row_number() over (partition by phone_number) as rw_end from call_end_logs) as en
on srt.phone_number = en.phone_number and rw_start = rw_end  ```