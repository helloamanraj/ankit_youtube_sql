Pivoting Technique problem

```sql
INSERT INTO exams VALUES
(1, 'Ash', 1, 70),
(2, 'Ash',2, 77),
(3, 'Ash', 3, 71),
(4, 'Ash', 4, 70),
(5, 'Ket', 1, 89),
(6, 'Ket', 2, 87),
(7, 'Ket', 3, 88),
(8, 'Ket', 4, 89);
```
solution:

```sql
select name,
sum(case when exam=1 then score else 0 end ) exam1,
sum(case when exam=2 then score else 0 end ) exam2,
sum(case when exam=3 then score else 0 end ) exam3,
sum(case when exam=4 then score else 0 end ) exam4
from exams
group by name 

```