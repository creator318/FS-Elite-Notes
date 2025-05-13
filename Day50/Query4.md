#SQL 

Find the name of the customer who spent the most in total

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

| Name           | TotalSpent |
| -------------- | ---------- |
| Alice Johnson  | 1625.00    |
| George Clark   | 1200.00    |
| Bob Smith      | 1050.00    |
| Charlie Davis  | 1050.00    |
| Diana Williams | 750.00     |
| Ethan Brown    | 550.00     |
| Fiona Adams    | 250.00     | 

## Solution:

```sql
USE FS;
SELECT c.Name, SUM(o.TotalCost) as TotalSpent FROM Orders o NATURAL JOIN Customers c GROUP BY c.CustomerID ORDER BY TotalSpent DESC;
```