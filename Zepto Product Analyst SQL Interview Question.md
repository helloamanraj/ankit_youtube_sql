Question link: https://www.youtube.com/watch?v=xS6iq3d8Eb4&t=616s


```sql
create table numbers (n int);

insert into numbers values (1),(2),(3),(4),(5)
insert into numbers values (9)
```

Solution 1:

```sql
with recursive cte as (
select max(n) as n from numbers
union all
select n - 1 from cte
where n - 1 >= 1
)


select n2.n from cte as n1 
join cte as n2 on n1.n <= n2.n
order by n2.n;
```

Solution 2:
```sql
with recursive cte as (
select max(n) as n from numbers
union all
select n - 1 from cte
where n - 1 >= 1
)
select n2.n from numbers as n1
cross join numbers as n2 on n1.n <= n2.n
order by n2.n;
```

Solution 3:
```sql
with recursive cte as (
select n, 1 as num_count from numbers
union all
select n, num_count + 1 from cte
where num_count+ 1 <= n
)

select n from cte
order by n
```