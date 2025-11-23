# SQL Practice Question Solutions

>**Please make sure you attempt the questions on your own before looking at these solutions.**
>To view the full list of questions, check the documentation here:
>👉 [View Documentation](https://medium.com/@ezeliorafavour/a-beginner-friendly-guide-to-ctes-common-table-expressions-in-postgresql-bf5f31416b7a)
---
Solution 1: Add a Row Number for the Entire Table
```
SELECT
    employee,
    region,
    amount,
    ROW_NUMBER() OVER (ORDER BY amount) AS row_num
FROM sales;
```
---
Solution 2: Show Each Sale With Total Sales for the Entire Table
```
SELECT
    employee,
    region,
    amount,
    SUM(amount) OVER () AS total_sales
FROM sales;
```
---
Solution 3: Show Each Employee With the Average Sale Across the Table
```
SELECT
    employee,
    region,
    amount,
    AVG(amount) OVER () AS avg_sale
FROM sales;
```
---
Solution 4: Number Employees Within Each Region (Highest to Lowest Sale)
```
SELECT
    employee,
    region,
    amount,
    ROW_NUMBER() OVER (PARTITION BY region ORDER BY amount DESC) AS row_num
FROM sales;
```
---
Solution 5: Running Total of Sales Within Each Region
```
SELECT
    employee,
    region,
    amount,
    SUM(amount) OVER (PARTITION BY region ORDER BY amount) AS running_total
FROM sales;
```
---
Solution 6: Previous Sale in the Same Region (Using LAG)
```
SELECT
    employee,
    region,
    amount,
    LAG(amount) OVER (PARTITION BY region ORDER BY amount) AS previous_amount
FROM sales;
```
---
Solution 7: Next Sale in the Same Region (Using LEAD)
```
SELECT
    employee,
    region,
    amount,
    LEAD(amount) OVER (PARTITION BY region ORDER BY amount) AS next_amount
FROM sales;
```
---
Solution 8: Employees Above Their Region’s Average
```
SELECT *
FROM (
    SELECT
        employee,
        region,
        amount,
        AVG(amount) OVER (PARTITION BY region) AS region_avg
    FROM sales
) AS sub
WHERE amount > region_avg;
```
---
Solution 9: Difference Between Each Employee’s Sale and Region Top Sale
```
SELECT
    employee,
    region,
    amount,
    FIRST_VALUE(amount) OVER (PARTITION BY region ORDER BY amount DESC) - amount AS diff_from_top
FROM sales;
```
---
Solution 10: Top 2 Employees by Sales Within Each Region
```
SELECT *
FROM (
    SELECT
        employee,
        region,
        amount,
        ROW_NUMBER() OVER (PARTITION BY region ORDER BY amount DESC) AS row_num
    FROM sales
```
) AS sub
WHERE row_num <= 2;
