Question 41:
https://www.youtube.com/watch?v=3qEfsSC27_4&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=41


Walmart Labs SQL Interview Question:
```sql create table phonelog(
    Callerid int,
    Recipientid int,
    Datecalled datetime
);


insert into phonelog(Callerid, Recipientid, Datecalled)
values(1, 2, '2019-01-01 09:00:00.000'),
       (1, 3, '2019-01-01 17:00:00.000'),
       (1, 4, '2019-01-01 23:00:00.000'),
       (2, 5, '2019-07-05 09:00:00.000'),
       (2, 3, '2019-07-05 17:00:00.000'),
       (2, 3, '2019-07-05 17:20:00.000'),
       (2, 5, '2019-07-05 23:00:00.000'),
       (2, 3, '2019-08-01 09:00:00.000'),
       (2, 3, '2019-08-01 17:00:00.000'),
       (2, 5, '2019-08-01 19:30:00.000'),
       (2, 4, '2019-08-02 09:00:00.000'),
       (2, 5, '2019-08-02 10:00:00.000'),
       (2, 5, '2019-08-02 10:45:00.000'),
       (2, 4, '2019-08-02 11:00:00.000');
       ```

Solution:

```sql  with cte as (

SELECT *
      ,  first_value(recipientid) over(partition by cast(datecalled as date) order by datecalled) as fcall
      ,  first_value(recipientid) over(partition by cast(datecalled as date) order by datecalled desc) as lcall
from phonelog

)

select distinct callerid, cast(datecalled as date) , fcall as recipientid from cte
where fcall = lcall
```