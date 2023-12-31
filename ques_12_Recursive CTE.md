Question 12:
https://www.youtube.com/watch?v=ewmEHQSQYRM&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=12

Recursive CTE Leetcode Hard SQL Problem 5:
```sql
create table sales (
product_id int,
period_start date,
period_end date,
average_daily_sales int
);

insert into sales values(1,'2019-01-25','2019-02-28',100),(2,'2018-12-01','2020-01-01',10),(3,'2019-12-01','2020-01-31',1);


```
Solution: 
```sql
WITH RECURSIVE cte AS (
   select min(period_start) as dates, max(period_end) as max_date from sales
    union all
    
   select c.dates + interval 1 day  , c.max_date from cte as c
   where  c.dates + INTERVAL 1 DAY < c.max_date
   
)
SELECT product_id, sum(average_daily_sales) as total_sales , year(dates) as yr FROM cte as c
inner join sales on dates between period_start and period_end
group by product_id, year(dates)
order by product_id

```
Another Solution without the use of recursion

```sql

SELECT
    b.product_id,
    a.yr AS report_year,
    CASE 
        WHEN YEAR(b.period_start)=YEAR(b.period_end) AND a.yr=YEAR(b.period_start) THEN DATEDIFF(b.period_end,b.period_start)+1
        WHEN a.yr=YEAR(b.period_start) THEN DATEDIFF(DATE_FORMAT(b.period_start,'%Y-12-31'),b.period_start)+1
        WHEN a.yr=YEAR(b.period_end) THEN DAYOFYEAR(b.period_end) 
        WHEN a.yr>YEAR(b.period_start) AND a.yr<YEAR(b.period_end) THEN 365
        ELSE 0
    END * average_daily_sales AS total_amount
FROM
    (SELECT product_id, '2018' AS yr FROM sales
    UNION
    SELECT product_id, '2019' AS yr FROM sales
    UNION
    SELECT product_id,'2020' AS yr FROM sales) a
    JOIN 
    Sales b
    ON a.product_id=b.product_id  
HAVING total_amount > 0
ORDER BY b.product_id,a.yr;


```