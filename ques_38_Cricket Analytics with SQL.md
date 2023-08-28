Question 38:
https://www.youtube.com/watch?v=7LufPVm01NQ&list=PLBTZqjSKn0IeKBQDjLmzisazhqQy4iGkb&index=38


Cricket Analytics with SQL:

Dataset link: https://docs.google.com/spreadsheets/d/1-utCWJ4PseJjLipW15Gm9lVG5J6FrmOR/edit?rtpof=true&sd=true&pli=1#gid=390450519


Solution:
```sql
WITH cte1 AS (
  SELECT *, SUM(Runs) OVER (ORDER BY innings ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS totalruns
  FROM sachin_batting_scores
 
)
,
 cte2 as (
 
  (SELECT *, 1000 AS milestone_runs
  FROM cte1
  WHERE totalruns > 1000
  ORDER BY totalruns LIMIT 1)
  UNION
  (SELECT *, 5000 AS milestone_runs
  FROM cte1
  WHERE totalruns > 5000
  ORDER BY totalruns LIMIT 1)
  UNION
  (SELECT *, 10000 AS milestone_runs
  FROM cte1
  WHERE totalruns > 10000
  ORDER BY totalruns LIMIT 1)
  union
  (select *, 15000 As milestone_rune from cte1
  where totalruns > 15000
  order by totalruns limit 1)
)

select ROW_NUMBER()over(order by Mat_no) as milestone_number,
milestone_runs,Innings as miliestone_Innings,
Mat_no as milestone_match_number, totalruns as actual_runs from cte2
```