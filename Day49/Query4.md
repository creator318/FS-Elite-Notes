#SQL 

Find all orders placed by customers living in "Texas (TX)".Include customer name, order date, total cost, and status.

Customers:
==================

| Field       | Type          | Null | Key | Default | Extra |
| ----------- | ------------- | ---- | --- | ------- | ----- |
| CustomerID  | int           | NO   | PRI | NULL    |       |
| Name        | varchar (255) | YES  |     | NULL    |       |
| Email       | varchar (255) | YES  |     | NULL    |       |
| Address     | varchar (255) | YES  |     | NULL    |       |
| PhoneNumber | varchar (20)  | YES  |     | NULL    |       |

Orders:
=======

| Field      | Type           | Null | Key | Default | Extra |
| ---------- | -------------- | ---- | --- | ------- | ----- |
| OrderID    | int            | NO   | PRI | NULL    |       |
| CustomerID | int            | YES  | MUL | NULL    |       |
| OrderDate  | date           | YES  |     | NULL    |       |
| TotalCost  | decimal (10,2) | YES  |     | NULL    |       |
| Status     | varchar (20)   | YES  |     | NULL    |       |

OrderItems:
============

| Field       | Type           | Null | Key | Default | Extra |
| ----------- | -------------- | ---- | --- | ------- | ----- |
| OrderItemID | int            | NO   | PRI | NULL    |       |
| OrderID     | int            | YES  | MUL | NULL    |       |
| ProductID   | int            | YES  | MUL | NULL    |       |
| Quantity    | int            | YES  |     | NULL    |       |
| UnitPrice   | decimal (10,2) | YES  |     | NULL    |       |

Products:
=========

| Field       | Type           | Null | Key | Default | Extra |
| ----------- | -------------- | ---- | --- | ------- | ----- |
| ProductID   | int            | NO   | PRI | NULL    |       |
| Name        | varchar (255)  | YES  |     | NULL    |       |
| Description | varchar (255)  | YES  |     | NULL    |       |
| Price       | decimal (10,2) | YES  |     | NULL    |       |

Sample Output:
==============

| Name           | OrderDate  | TotalCost | Status     |
| -------------- | ---------- | --------- | ---------- |
| Diana Williams | 2024-10-13 | 450.00    | Shipped    |
| Diana Williams | 2024-10-14 | 300.00    | Processing |

## Solution:

```sql
USE FS;
SELECT c.Name, o.OrderDate, o.TotalCost, o.Status From Customers c NATURAL JOIN Orders o WHERE c.Address LIKE "%TX%";
```