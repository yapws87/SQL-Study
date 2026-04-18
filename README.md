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
### Ranking

```
SELECT * FROM()
```
