# SQL-Study
A self study with LLM to learn basics of SQL

# Basics

## Syntax orders
These are the orders from which every SQL query should follow
```
SELECT   -- 1. Which columns do I want to see?
FROM     -- 2. Where is the main data stored?
JOIN     -- 3. What other table do I need to pull in?
ON       -- 4. How are those tables connected?
WHERE    -- 5. Filter individual rows (e.g., age > 21)
GROUP BY -- 6. Collapse rows into groups (e.g., by country)
HAVING   -- 7. Filter the groups (e.g., count > 10)
ORDER BY -- 8. Sort the final output
LIMIT    -- 9. Only show the top X results
```

## Templates

### The "Top N" Ranking
Find the top 3 highest spending customer in each region
```
SELECT * FROM(
  SELECT name, region,
  RANK() OVER (PARTITION BY region ORDER BY spend DESC) as rank_val
  FROM sales
) temp
WHERE rank_val <=3;
```

SELECT * FROM(
  SELECT *, 
    RANK() OVER(PARTITION BY region ORDER BY sales DESC) rank_sales
  FROM table
) temp
WHERE [] > 3;



### The "Difference/Mission" Pattern
Which product has never been ordered
```
Select p.name 
FROM Products p
LEFT JOIN Orders o ON p.product_id = o.product_id
WHERE o.order_id IS NULL
```

### The "Time Series" Pattern
Find users who are active today and yesterday
```
SELECT a.user_id
FROM Activity a
JOIN Activity b on a.user_id = b.user_id
WHERE a.date = '2026-04-16'
AND b.date = '2026-04-15'
```

For more dates

```
SELECT user_id
FROM Activity
WHERE date IN ('2026-04-16','2026-04-15','2026-04-14')
GROUP BY user_id
HAVING COUNT(DISTINCT date) = 3;
```

For consecutive days
```
WITH Ranked AS(
  SELECT user_id, date,
    
)
```

### Notes
1) Strings use single quotes. 'strings' and not "strings"
2) COUNT(*) counts every row; COUNT(column) only counts non-null values; COUNT(DISTINCT user) to count uniques
