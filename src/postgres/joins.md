# Postgres JOINS (Customer & Orders Example with PK/FK)

## Learning Objectives

Able to...

1. Understand INNER, LEFT, RIGHT, and FULL JOINS in PostgreSQL

---

## Model 1

**Customers**

| customer_id | customer_name |
| ----------- | ------------- |
| c1          | Alice         |
| c2          | Bob           |
| c3          | Carol         |
| c4          | David         |

**Orders**

| order_id | customer_id |
| -------- | ----------- |
| o1       | c1          |
| o2       | c2          |
| o5       | c5          |
| o6       | c6          |

Questions

1. Which customers have matching orders?
    c1 c2
2. Which customers do not have any orders?
    c3 c4
3. Which orders do not match any existing customers?
    c5 c6

---

## Model 2

Query

```sql
SELECT c.customer_id, c.customer_name, o.order_id
FROM Customers c
INNER JOIN Orders o ON c.customer_id = o.customer_id;
```

Result
| customer_id | customer_name | order_id |
| ----------- | ------------- | -------- |
| c1	        | Alice	        | o1       |
| c2	        | Bob	          | o2       |

Questions

1. Which customers are not included in the result?
    c3 c4
2. Why do you think they are not in the result?
   Because inner join only returns rows where there is a match in both tables
3. Which orders are not included in the result?
   c5 c6
4. Why do you think they are not in the result?
   They do not exist in the customer tables
5. When is a row from Customers or Orders included?
   Its when the customer_id exists in both customers and orders
6. What is the meaning of INNER JOIN?
   Inner join returns only the rows that have matching values in both tables, excluding non-matching rows from either side.

---

## Model 3

Query

```sql
SELECT c.customer_id, c.customer_name, o.order_id
FROM Customers c
LEFT OUTER JOIN Orders o ON c.customer_id = o.customer_id;
```

Result
| customer_id | customer_name | order_id |
| ----------- | ------------- | -------- |
| c1          | Alice	        | o1       |
| c2	        | Bob	          | o2       |
| c3	        | Carol	        |          |
| c4	        | David	        |          |

Questions

1. Which customers are not included in the result?
   All are included
2. Which orders are not included in the result?
    o5 o6 or orders without a valid customer_id are not included.
3. When is a row included?
   A row is included if there is a customer id in the Customers table
4. What is the meaning of LEFT OUTER JOIN?
   A LEFT OUTER JOIN returns all rows from the left table (Customers) and the matching rows from the right table (Orders).

---

## Model 4

Query

```sql
SELECT c.customer_id, c.customer_name, o.order_id
FROM Customers c
RIGHT OUTER JOIN Orders o ON c.customer_id = o.customer_id;
```

Result
| customer_id | customer_name | order_id |
| ----------- | ------------- | -------- |
| c1          | Alice	        | o1       |
| c2	        | Bob	          | o2       |
|   	        |      	        | o5       |
|   	        |     	        | o6       |


Questions

1. Which customers are not included in the result?
   c3 c4
2. Which orders are not included in the result?
    This is a RIGHT OUTER JOIN, meaning all orders from the Orders table are included, even if they don’t have a matching customer.
3. When is a row included?
   A row is included if it exists in the table.
4. What is the meaning of RIGHT OUTER JOIN?
   A RIGHT OUTER JOIN returns all rows from the right table (Orders) and the matching rows from the left table

---

## Model 5

Query

```sql
SELECT c.customer_id, c.customer_name, o.order_id
FROM Customers c
FULL JOIN Orders o ON c.customer_id = o.customer_id;
```

Result
| customer_id | customer_name | order_id |
| ----------- | ------------- | -------- |
| c1          | Alice	        | o1       |
| c2	        | Bob	          | o2       |
| c3	        | Carol	        |          |
| c4	        | David	        |          |
|   	        |      	        | o5       |
|   	        |     	        | o6       |

Questions

1. Which customers are not included in the result?
   All are included
2. Which orders are not included in the result?
   All orders are included
3. When is a row included?
   If a customer or an order is included in the customers or the orders table respectively
4. What is the meaning of FULL JOIN?
   FULL JOIN returns all rows from both tables.

---

## Model 6

Confirm the above by creating the tables in Postgres and running the queries. Paste the SQL for creating and populating the tables below.

```sql
-- Clean start
DROP TABLE IF EXISTS orders;
DROP TABLE IF EXISTS customers;

-- 1) Tables
CREATE TABLE customers (
  customer_id  TEXT PRIMARY KEY,
  customer_name TEXT NOT NULL
);

CREATE TABLE orders (
  order_id     TEXT PRIMARY KEY,
  customer_id  TEXT  -- allow NULL so we can have orders with no customer
);

-- 2) Sample data
INSERT INTO customers (customer_id, customer_name) VALUES
  ('c1','Alice'),
  ('c2','Bob'),
  ('c3','Carol'),
  ('c4','David');

-- o5 and o6 have no matching customer to demonstrate RIGHT/FULL joins
INSERT INTO orders (order_id, customer_id) VALUES
  ('o1','c1'),
  ('o2','c2'),
  ('o5',NULL),
  ('o6',NULL);

-- 3) Confirm the joins

-- LEFT OUTER JOIN (all customers; orders if present)
SELECT c.customer_id, c.customer_name, o.order_id
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
ORDER BY c.customer_id, o.order_id;

-- RIGHT OUTER JOIN (all orders; customers if present)
SELECT c.customer_id, c.customer_name, o.order_id
FROM customers c
RIGHT JOIN orders o ON c.customer_id = o.customer_id
ORDER BY c.customer_id NULLS LAST, o.order_id;

-- FULL OUTER JOIN (all customers + all orders)
SELECT c.customer_id, c.customer_name, o.order_id
FROM customers c
FULL JOIN orders o ON c.customer_id = o.customer_id
ORDER BY c.customer_id NULLS LAST, o.order_id;

```
