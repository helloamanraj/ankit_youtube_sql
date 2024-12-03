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


Solution1:

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

Solution2:

```sql
with cte as (

select order_id, order_date,  i.item_id, item_brand, u.user_id as user_id, join_date, favorite_brand from users as u
left join orders as o on  U.user_id = o.seller_id
left join items as i on i.item_id = o.item_id
order by order_id

)
, cte2 as (
select distinct User_id, case when item_brand = favorite_brand  then "Yes" else "No" end as Item_fav_brand
from (select *, rank() over (partition by user_id order by order_date ) as rnk from cte) as x
where rnk = 2)

select * from cte2

union all

select user_id , "No" from users
where user_id not in (select user_id from cte2)
```