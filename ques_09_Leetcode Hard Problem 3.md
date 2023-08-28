Question 09:
https://www.youtube.com/watch?v=1ias-sP_XAY&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=9


Leetcode Hard Problem 3 | Market Analysis 2


```sql

create table users (
user_id         int     ,
 join_date       date    ,
 favorite_brand  varchar(50));

drop table orders
 create table orders (
 order_id       int     ,
 order_date     date    ,
 item_id        int     ,
 buyer_id       int     ,
 seller_id      int
 );


 create table items
 (
 item_id        int     ,
 item_brand     varchar(50)
 );


 insert into users values (1,'2019-01-01','Lenovo'),(2,'2019-02-09','Samsung'),(3,'2019-01-19','LG'),(4,'2019-05-21','HP');

 insert into items values (1,'Samsung'),(2,'Lenovo'),(3,'LG'),(4,'HP');

 insert into orders values (1,'2019-08-01',4,1,2),(2,'2019-08-02',2,1,3),(3,'2019-08-03',3,2,3),(4,'2019-08-04',1,4,2)
 ,(5,'2019-08-04',1,3,4),(6,'2019-08-05',2,2,4);
```


Solution:

```sql
with cte as (
 select o.*,i.item_brand,u.*,
        row_number() over (partition by seller_id order by order_date) as rw,
        case when (row_number() over (partition by seller_id order by order_date) = 2 and favorite_brand = item_brand) then 'Yes' else 'No' end as item_fav
 from orders as o
 right join users as u on o.seller_id = u.user_id
 left join items as i on o.item_id = i.item_id
 )
 
SELECT DISTINCT user_id,
       CASE WHEN SUM(CASE WHEN rw = 2 AND item_fav = 'Yes' THEN 1 ELSE 0 END) > 0 THEN 'Yes' ELSE 'No' END AS p
FROM cte
GROUP BY user_id;```