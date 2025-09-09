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
   o3 o4
3. When is a row included?
   A row is included for every customer from the Customers table, regardless of whether they have orders or not.
4. What is the meaning of LEFT OUTER JOIN?
   LEFT OUTER JOIN returns all records from the left table (Customers), and the matched records from the right table (Orders). If there is no match, the result will contain NULL values for the right table columns.

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
2. Which orders are not included in the result?
3. When is a row included?
4. What is the meaning of RIGHT OUTER JOIN?

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
2. Which orders are not included in the result?
3. When is a row included?
4. What is the meaning of FULL JOIN?

---

## Model 6

Confirm the above by creating the tables in Postgres and running the queries. Paste the SQL for creating and populating the tables below.

```sql

```
