Question 50:
https://www.youtube.com/watch?v=X6i1WMx0vnY&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=50


Fintech Startup Sql Question


```sql
Create Table Trade_tbl(
TRADE_ID varchar(20),
Trade_Timestamp time,
Trade_Stock varchar(20),
Quantity int,
Price Float
)

INSERT INTO Trade_tbl VALUES ('TRADE1', '10:01:05', 'ITJunction4All', 100, 20);
INSERT INTO Trade_tbl VALUES ('TRADE2', '10:01:06', 'ITJunction4All', 20, 15);
INSERT INTO Trade_tbl VALUES ('TRADE3', '10:01:08', 'ITJunction4All', 150, 30);
INSERT INTO Trade_tbl VALUES ('TRADE4', '10:01:09', 'ITJunction4All', 300, 32);
INSERT INTO Trade_tbl VALUES ('TRADE5', '10:10:00', 'ITJunction4All', -100, 19);
INSERT INTO Trade_tbl VALUES ('TRADE6', '10:10:01', 'ITJunction4All', -300, 19);
```

Solution:

```sql
with cte as (
SELECT a.Trade_id as first_trade, b.trade_id as second_trade , TIMESTAMPDIFF( second, (a.trade_timestamp) ,(b.trade_timestamp))  as time_diff
,round((abs(a.price - b.price)/a.price) *100,0) as price_per
FROM trade_tbl AS a
CROSS JOIN trade_tbl AS b
where a.Trade_Timestamp < b.trade_timestamp
order by a.trade_id, b.trade_id
)

select * from cte
where price_per > 10 and time_diff < 10
```