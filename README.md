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

### 1) The "Top N" Ranking
Find the top 3 highest spending customers in each region
```SQL
SELECT * FROM(
  SELECT name, region,
  RANK() OVER (PARTITION BY region ORDER BY spend DESC) as rank_val
  FROM sales
) temp
WHERE rank_val <=3;
```

### 2) The "Difference/Mission" Pattern
Which product has never been ordered
```SQL
Select p.name 
FROM Products p
LEFT JOIN Orders o ON p.product_id = o.product_id
WHERE o.order_id IS NULL
```

### 3) The "Time Series" Pattern
Find users who are active today and yesterday
```SQL
SELECT a.user_id
FROM Activity a
JOIN Activity b on a.user_id = b.user_id
WHERE a.date = '2026-04-16'
AND b.date = '2026-04-15'
```

For more dates

```SQL
SELECT user_id
FROM Activity
WHERE date IN ('2026-04-16','2026-04-15','2026-04-14')
GROUP BY user_id
HAVING COUNT(DISTINCT date) = 3;
```

For consecutive days
```SQL
WITH Ranked AS(
  SELECT user_id, date,
    
)
```

## Example Questions

### 1) Power Users
- The Problem: Co. wants to find "Power Users" for their "App" team.
- You have two tables:
  - Users (user_id, name, city) and
  - Usage (usage_id, user_id, minutes_used, app_name).
- Your Task: Find the top 2 users in each city who have the highest total usage time across all apps.

Constraints:

- Only include users who have used at least 3 different apps.
- Return the results sorted by City (A-Z) and then by their Rank (1 to 2).

Solution:
```SQL
WITH UserUsage AS (
    -- Get total time AND count of distinct apps per user
    SELECT user_id, 
           SUM(minutes_used) AS total_time, 
           COUNT(DISTINCT app_name) AS unique_apps
    FROM Usage
    GROUP BY user_id
    HAVING COUNT(DISTINCT app_name) >= 3 -- Constraint: At least 3 apps
),
RankedUsers AS (
    -- Join with Users and calculate Rank
    SELECT u.name, u.city, 
           RANK() OVER (PARTITION BY u.city ORDER BY uu.total_time DESC) as rank_val
    FROM Users u
    JOIN UserUsage uu ON u.user_id = uu.user_id
)
SELECT name, city
FROM RankedUsers
WHERE rank_val <= 2
ORDER BY city ASC, rank_val ASC;
```




### Notes
1) Strings use single quotes. 'strings' and not "strings"
2) COUNT(*) counts every row; COUNT(column) only counts non-null values; COUNT(DISTINCT user) to count uniques
