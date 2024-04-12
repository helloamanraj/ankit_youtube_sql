Question: https://www.youtube.com/watch?v=tVQUsozKkyI


Solution:
```sql

with cte as (
select customer_name,
(length(customer_name)  - length(replace(customer_name, ' ', '')) + 1) as cnt_of_words from customers
)

SELECT 
    CASE 
        WHEN cnt_of_words = 1 THEN customer_name -- Only one word, consider it as the first name
        WHEN cnt_of_words = 2 THEN SUBSTRING_INDEX(customer_name, ' ', 1) -- Two words, consider the first word as first name
        WHEN cnt_of_words = 3 THEN SUBSTRING_INDEX(customer_name, ' ', 1) -- Three words, consider the first word as first name
    END AS first_name,
    CASE 
        WHEN cnt_of_words = 3 THEN SUBSTRING_INDEX(SUBSTRING_INDEX(customer_name, ' ', 2), ' ', -1) -- Three words, consider the second word as middle name
    END AS middle_name,
    CASE 
        WHEN cnt_of_words >= 2 THEN SUBSTRING_INDEX(customer_name, ' ', -1) -- At least two words, consider the last word as last name
    END AS last_name
FROM 
    cte;
 ```   
