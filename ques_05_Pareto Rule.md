Question 05:
https://www.youtube.com/watch?v=oGgE180oaTs&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=5





Complex SQL 5:

Dataset: https://drive.google.com/drive/folders/1Dc81McsB4lp1JUIwssDmmOaw6Z7rBK8T


```sql
ALTER TABLE you_tube_data.superstore_orders
CHANGE COLUMN `ï»¿Row_ID` row_id INT; -- Adjust the data type as needed



with cte as(
Select product_id,SUM(sales) sales from superstore_orders group by product_id),
cte1 as(
Select *,round(SUM(sales) over (order by sales desc rows between unbounded preceding and current row),2)as running_sales  from cte)
Select * from cte1 where running_sales<=(Select round(0.8*SUM(Sales),2) sales from superstore_orders);
```