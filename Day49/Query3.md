#SQL 

Show product names along with the total quantity sold for each product. Display only products where the total quantity sold is greater than or equal to 2.

Customers:
==================

| Field       | Type         | Null | Key | Default | Extra |
| ----------- | ------------ | ---- | --- | ------- | ----- |
| CustomerID  | int          | NO   | PRI | NULL    |       |
| Name        | varchar(255) | YES  |     | NULL    |       |
| Email       | varchar(255) | YES  |     | NULL    |       |
| Address     | varchar(255) | YES  |     | NULL    |       |
| PhoneNumber | varchar(20)  | YES  |     | NULL    |       |

Orders:
=======

| Field      | Type          | Null | Key | Default | Extra |
| ---------- | ------------- | ---- | --- | ------- | ----- |
| OrderID    | int           | NO   | PRI | NULL    |       |
| CustomerID | int           | YES  | MUL | NULL    |       |
| OrderDate  | date          | YES  |     | NULL    |       |
| TotalCost  | decimal(10,2) | YES  |     | NULL    |       |
| Status     | varchar(20)   | YES  |     | NULL    |       |

OrderItems:
============

| Field       | Type          | Null | Key | Default | Extra |
| ----------- | ------------- | ---- | --- | ------- | ----- |
| OrderItemID | int           | NO   | PRI | NULL    |       |
| OrderID     | int           | YES  | MUL | NULL    |       |
| ProductID   | int           | YES  | MUL | NULL    |       |
| Quantity    | int           | YES  |     | NULL    |       |
| UnitPrice   | decimal(10,2) | YES  |     | NULL    |       |

Products:
=========

| Field       | Type          | Null | Key | Default | Extra |
| ----------- | ------------- | ---- | --- | ------- | ----- |
| ProductID   | int           | NO   | PRI | NULL    |       |
| Name        | varchar(255)  | YES  |     | NULL    |       |
| Description | varchar(255)  | YES  |     | NULL    |       |
| Price       | decimal(10,2) | YES  |     | NULL    |       |

Sample Output:
==============

| ProductName | TotalQuantitySold |
| ----------- | ----------------- |
| Laptop      | 2                 |
| Keyboard    | 5                 |
| Smartphone  | 2                 |
| Mouse       | 8                 |
| Smartwatch  | 2                 |
| Monitor     | 2                 | 

## Solution:

```sql
USE FS;
SELECT p.Name AS ProductName, SUM(o.quantity) AS TotalQuantitySold FROM Products p NATURAL JOIN OrderItems o GROUP BY p.Name HAVING TotalQuantitySold > 1;
```