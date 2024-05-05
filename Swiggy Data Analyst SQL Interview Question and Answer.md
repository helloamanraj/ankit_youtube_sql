Question: https://www.youtube.com/watch?v=l72hohmWA68
```sql
CREATE TABLE stock (
    supplier_id INT,
    product_id INT,
    stock_quantity INT,
    record_date DATE
);


INSERT INTO stock (supplier_id, product_id, stock_quantity, record_date)
VALUES
    (1, 1, 60, '2022-01-01'),
    (1, 1, 40, '2022-01-02'),
    (1, 1, 35, '2022-01-03'),
    (1, 1, 45, '2022-01-04'),
    (1, 1, 51, '2022-01-06'),
    (1, 1, 55, '2022-01-09'),
    (1, 1, 25, '2022-01-10'),
    (1, 1, 48, '2022-01-11'),
    (1, 1, 45, '2022-01-15'),
    (1, 1, 38, '2022-01-16'),
    (1, 2, 45, '2022-01-08'),
    (1, 2, 40, '2022-01-09'),
    (2, 1, 45, '2022-01-06'),
    (2, 1, 55, '2022-01-07'),
    (2, 2, 45, '2022-01-08'),
    (2, 2, 48, '2022-01-09'),
    (2, 2, 35, '2022-01-10'),
    (2, 2, 52, '2022-01-15'),
    (2, 2, 23, '2022-01-16');

```

Solution1:
```sql
 with cte as (
 select *, lead(record_date, 1) over (partition by supplier_id, product_id order by record_date) as next_date,
 lead(record_date, 2) over (partition by supplier_id, product_id order by record_date) as next_to_date
 from stock 
 where stock_quantity < 50
 )
 
 select supplier_id, product_id, record_date as first_date from cte
 where datediff(next_date, record_date) = 1 and datediff( next_to_date, record_date) = 2
 ```

 Solution2:

 ```sql
 with cte as (
 select *,row_number() over (partition by supplier_id, product_id order by record_date) as rw
 from stock 
 where stock_quantity < 50
 )
 ,cte2 as (
 select *, date_add(record_date, interval - rw day) as grouped from cte
 )
 
 select supplier_id, product_id, min(record_date) as first_date from (select *, count(grouped) over (partition by supplier_id, product_id, grouped)as cnt from cte2)  x
where cnt > 2
group by supplier_id, product_id
```