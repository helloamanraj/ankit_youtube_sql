Question 28:
https://www.youtube.com/watch?v=e1SVjR-xoto&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=28

Brilliant SQL Interview Question:

```sql
CREATE TABLE int_orders (
    order_number INT NOT NULL,
    order_date DATE NOT NULL,
    cust_id INT NOT NULL,
    salesperson_id INT NOT NULL,
    amount FLOAT NOT NULL
);

INSERT INTO int_orders (order_number, order_date, cust_id, salesperson_id, amount) VALUES (30, CAST('1995-07-14' AS Date), 9, 1, 460);
INSERT INTO int_orders (order_number, order_date, cust_id, salesperson_id, amount) VALUES (10, CAST('1996-08-02' AS Date), 4, 2, 540);
INSERT INTO int_orders (order_number, order_date, cust_id, salesperson_id, amount) VALUES (40, CAST('1998-01-29' AS Date), 7, 2, 2400);
INSERT INTO int_orders (order_number, order_date, cust_id, salesperson_id, amount) VALUES (50, CAST('1998-02-03' AS Date), 6, 7, 600);
INSERT INTO int_orders (order_number, order_date, cust_id, salesperson_id, amount) VALUES (60, CAST('1998-03-02' AS Date), 6, 7, 720);
INSERT INTO int_orders (order_number, order_date, cust_id, salesperson_id, amount) VALUES (70, CAST('1998-05-06' AS Date), 9, 7, 150);
INSERT INTO int_orders (order_number, order_date, cust_id, salesperson_id, amount) VALUES (20, CAST('1999-01-30' AS Date), 4, 8, 1800);```

Solution: 

```sql
select a.* from int_orders as a
join int_orders as b on a.salesperson_id = b.salesperson_id
group by 1,2,3,4
having a.amount >= max(b.amount)
```