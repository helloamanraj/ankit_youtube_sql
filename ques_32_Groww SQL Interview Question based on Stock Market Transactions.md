Question 32:
https://www.youtube.com/watch?v=UfL-dMxDOew&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=32

Groww SQL Interview Question based on Stock Market Transactions:

```sql
Create Table Buy (
Date Int,
Time Int,
Qty Int,
per_share_price int,
total_value int )

drop table sell
Create Table Sell(
Date Int,
Time Int,
Qty Int,
per_share_price int,
total_value int );
INSERT INTO Buy (date, time, qty, per_share_price, total_value)
VALUES
(15, 10, 10, 10, 100),
(15, 14, 20, 10, 200);

INSERT INTO Sell(date, time, qty, per_share_price, total_value)
VALUES (15, 13, 8, 20, 240), (15, 15, 12, 20, 160), (15, 16, 5, 20, 160) ;
```

Solution: 

```sql
with running_sum_values as(

SELECT
    buy.time AS buy_time,
    buy.qty AS buy_qty,
    sell.qty AS sell_qty,
    SUM(buy.qty) OVER (ORDER BY buy.time) AS r_buy_qty,
    COALESCE(SUM(buy.qty) OVER (ORDER BY buy.time ROWS BETWEEN UNBOUNDED PRECEDING AND 1 PRECEDING), 0) AS r_buy_qty_prev
FROM
    buy
INNER JOIN
    sell ON buy.date = sell.date AND buy.time < sell.time
)

SELECT buy_time,
       CASE WHEN sell_qty >= r_buy_qty THEN buy_qty
            ELSE sell_qty - r_buy_qty_prev
       END AS buy_qty,
       CASE WHEN sell_qty >= r_buy_qty THEN buy_qty
            ELSE sell_qty - r_buy_qty_prev
       END AS sell_qty
FROM running_sum_values
WHERE sell_qty >= r_buy_qty

UNION ALL

SELECT buy_time,
       r_buy_qty - sell_qty AS buy_qty,
       NULL AS sell_qty
FROM running_sum_values
WHERE sell_qty < r_buy_qty;```
