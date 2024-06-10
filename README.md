# Restaurant-Order-Analysis
Restaurant Order Analysis - Maven Analytics by Alice Zhao

Guided project from Maven Analytics (Alice Zhao). 
<https://app.mavenanalytics.io/guided-projects/d7167b45-6317-49c9-b2bb-42e2a9e9c0bc>

## Project brief

Please refer to the Maven Analytics page for the project video. 

## Dataset 

**Restaurant orders ** <https://maven-datasets.s3.amazonaws.com/Restaurant+Orders/Restaurant+Orders+MySQL.zip> 
A quarter's worth of orders from a fictitious international cuisine restaurant.

## Useful tools

- Open-source relational database management system: MySQL <https://www.mysql.com/>.
- Maven analytics website: <https://app.mavenanalytics.io/>.
- SQL Pocket guide by Alice Zhao <https://www.oreilly.com/library/view/sql-pocket-guide/9781492090397/>.
- SQL Tutorial <https://www.w3schools.com/sql/default.asp>.


## EXERCISES

### OBJECTIVE 1 - Explore the items table

Better understand the items table by finding the number of rows in the table, the least and most expensive items, and the item prices within each category.

-- **Explore the items table**

-- I select (SELECT) all the items from (FROM) the menu_items table to inspect the table's structure. 

``` sql
SELECT * 
FROM menu_items
;
```

-- **What are the least and most expensive items on the menu?**

-- I select (SELECT) all the items from (FROM) the menu_items table and I order (ORDER BY) them from least to most expensive(DESC)/least expensive(ASC). 

``` sql
SELECT *
FROM menu_items
ORDER BY price DESC
;
```

-- most expensive = "Shrimp Scampi"

```sql
SELECT *
FROM menu_items
ORDER BY price ASC
;
```

-- least expensive = "Edamame"

-- **How many Italian dishes are on the menu? What are the least and most expensive Italian dishes on the menu?**

-- I select (SELECT) all the items from (FROM) the menu_items table but only the one under the Italian category (WHERE). I then order (ORDER BY) them 
-- by price to find the least expensive (ASC)/most expensive(DESC) item . 

``sql 
SELECT *
FROM menu_items
WHERE category = 'Italian'  
ORDER BY price ASC 
; 
```

-- least expensive = "Fettuccine Alfredo" and "Spaghetti" (same price). 


```sql
SELECT *
FROM menu_items
WHERE category = 'Italian'
ORDER BY price DESC-- descendant order so to find the most expensive item. 
; 
```

-- most expensive = "Shrimp Scampi"

-- We could add LIMIT 1 in both scripts to limit the number of results, but in this case we wouldn't find out that two items shared the same price. 

-- **How many dishes are in each category? What is the average dish price within each category?**

-- I count all [COUNT(*)] dishes  from (FROM) the menu_items and I file them under a new column (AS "").  
-- I then group the results by category (GROUP BY).

```sql 
SELECT category, COUNT(*) AS NumDish 
FROM menu_items
GROUP BY category 
;
```

### OBJECTIVE 2 - Explore the orders table

Better understand the orders table by finding the date range, the number of items within each order, and the orders with the highest number of items.

-- **View the order_details table.**

```sql
SELECT *
FROM order_details;
```

-- **What is the date range of the table?**

I select (SELECT) all the items from (FROM) the order_details table and I order them (ORDER BY) by order_date and find the date range. 

```sql
SELECT *
FROM order_details
ORDER BY order_date;
```

-- range JANUARY-MARCH 2023 -- 

-- We can also isolate the range extremes by using SELECT MIN and MAX.

```sql 
SELECT MIN(order_date) AS order_min
FROM order_details; -- 2023-01-01

SELECT MAX(order_date) AS order_max 
FROM order_details; -- 2023-03-31
```

-- **How many orders were made within this date range?**  

-- We need to count the orders made within that date range

-- I select (SELECT) all unique orders from the order_details table (FROM) and count [COUNT(DISTINCT)] the number of orders in the order_id column. 

```sql
SELECT COUNT(DISTINCT order_id) as number_orders 
FROM order_details 
;
```
 
-- Result: 5370

-- **How many items were ordered within this date range?**

-- I select (SELECT) all orders from the order_details table (FROM) and count all the orders (COUNT).
-- We know already that the range is included in the column order_date. 

```sql
SELECT COUNT(*) 
FROM order_details
```

-- 12234

-- **Which orders had the most number of items?**

-- I select the orders (SELECT) in the order_id column in the order_details table (FROM) and count the items (COUNT) in a new column (AS num_items)
-- I then group them (GROUP BY) by their order_id and order the latter (ORDER BY) by the number of items ordered for each order. 

```sql
SELECT order_id, COUNT(item_id) AS num_items
FROM order_details
GROUP BY order_id
ORDER BY num_items DESC -- the order with most number of 
;
```

-- **How many orders had more than 12 items?**

-- This question involves writing a subquery. 

-- First query:
-- I select and count [SELECT COUNT(*)] all the orders in the order_id column in the order_details table (FROM).
-- I then group them (GROUP BY) by their order_id but only the ones with more than 12 items per order (HAVING .... > 12).

-- I then use this query to formulate a different one: 
-- I select the query I just wrote and treat it as a table [SELECT COUNT(*)]
-- and count all the orders that have more than 12 items 

```sql
SELECT COUNT(*)
FROM 
(SELECT order_id, COUNT(item_id)
FROM order_details
GROUP BY order_id
HAVING num_items > 12) AS num_orders -- tutti gli ordini con piu' di 12 items
;
```

--- result: 20


### Objective 3 - Analyze customer behavior

Combine the items and orders tables, find the least and most ordered categories, and dive into the details of the highest spend orders.



