#SQL 

Show customers who placed more orders than the average number of orders per customer

Customers:
==================

| Field       | Type         | Null | Key | Default | Extra |
| ----------- | ------------ | ---- | --- | ------- | ----- |
| CustomerID  | int          | NO   | PRI | NULL    |       |
| Name        | varchar (255) | YES  |     | NULL    |       |
| Email       | varchar (255) | YES  |     | NULL    |       |
| Address     | varchar (255) | YES  |     | NULL    |       |
| PhoneNumber | varchar (20)  | YES  |     | NULL    |       |

Orders:
=======

| Field      | Type          | Null | Key | Default | Extra |
| ---------- | ------------- | ---- | --- | ------- | ----- |
| OrderID    | int           | NO   | PRI | NULL    |       |
| CustomerID | int           | YES  | MUL | NULL    |       |
| OrderDate  | date          | YES  |     | NULL    |       |
| TotalCost  | decimal (10,2) | YES  |     | NULL    |       |
| Status     | varchar (20)   | YES  |     | NULL    |       |

OrderItems:
============

| Field       | Type          | Null | Key | Default | Extra |
| ----------- | ------------- | ---- | --- | ------- | ----- |
| OrderItemID | int           | NO   | PRI | NULL    |       |
| OrderID     | int           | YES  | MUL | NULL    |       |
| ProductID   | int           | YES  | MUL | NULL    |       |
| Quantity    | int           | YES  |     | NULL    |       |
| UnitPrice   | decimal (10,2) | YES  |     | NULL    |       |

Products:
=========

| Field       | Type          | Null | Key | Default | Extra |
| ----------- | ------------- | ---- | --- | ------- | ----- |
| ProductID   | int           | NO   | PRI | NULL    |       |
| Name        | varchar (255)  | YES  |     | NULL    |       |
| Description | varchar (255)  | YES  |     | NULL    |       |
| Price       | decimal (10,2) | YES  |     | NULL    |       |

Sample Output:
==============

| CustomerID | Name          | NumOrders |
| ---------- | ------------- | --------- |
| 1          | Alice Johnson | 4         | 
| 2          | Bob Smith     | 3         |
| 3          | Charlie Davis | 3         |

## Solution:

```sql
USE FS;
SELECT c.CustomerID, c.Name, COUNT(o.OrderID) AS NumOrders FROM Customers c NATURAL JOIN Orders o GROUP BY c.CustomerID HAVING NumOrders > (SELECT AVG(counts) FROM (SELECT COUNT(OrderID) AS counts FROM Orders NATURAL JOIN Customers GROUP BY CustomerID) AS avgcounts);
```