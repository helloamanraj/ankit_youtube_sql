Question 48:
https://www.youtube.com/watch?v=FNUIqQbj_EE&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=48



Amazon Sql Interview question:

```sql

CREATE TABLE purchase_history (
    userid INT,
    productid INT,
    purchasedate DATE
);

INSERT INTO purchase_history (userid, productid, purchasedate)
VALUES
    (1, 1, '2012-01-23'),
    (1, 2, '2012-01-23'),
    (1, 3, '2012-01-25'),
    (2, 1, '2012-01-23'),
    (2, 2, '2012-01-23'),
    (2, 2, '2012-01-25'),
    (2, 4, '2012-01-25'),
    (3, 4, '2012-01-23'),
    (3, 1, '2012-01-23'),
    (4, 1, '2012-01-23'),
    (4, 2, '2012-01-25');
```


Solution:

```sql
with cte as (

select userid, productid, count(distinct purchasedate) as dis_date , count(distinct productid) as dist_prod,
count(productid) count_prod from purchase_history
group by userid
having count(distinct purchasedate) > 1

)

select userid from cte
where dist_prod = count_prod```


Another Solution:
```sql
WITH criteria AS(
SELECT userid,
DENSE_RANK() OVER (PARTITION BY userid, productid ORDER BY purchasedate) as sameproduct,
DENSE_RANK() OVER (PARTITION BY userid ORDER BY purchasedate) as differentdays
FROM purchase_history
)
SELECT userid
FROM criteria
GROUP BY userid
HAVING MAX(sameproduct) = 1 AND MAX(differentdays) > 1
```