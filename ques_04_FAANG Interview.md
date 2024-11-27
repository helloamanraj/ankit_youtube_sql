Question 04:
https://www.youtube.com/watch?v=6XQAokp4UCs&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=4



FAANG Interview:
```sql
 set @given_date = '2024-11-27';
 set @n = 3;
 
select date_add(@given_date, interval(7 - dayofweek(@given_date) + 1 + 7 * (@n-1)) Day ) as nth_Sunday

```