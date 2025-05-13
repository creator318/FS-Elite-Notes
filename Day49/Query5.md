#SQL 

List orders that contain more than one item. Show OrderID, customer name, and the total number of items in the order.

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

| OrderID | CustomerName   | NumberOfItems |
| ------- | -------------- | ------------- |
| 1001    | Alice Johnson  | 2             |
| 1003    | Alice Johnson  | 2             |
| 1010    | Charlie Davis  | 2             |
| 1005    | Diana Williams | 2             |
| 1006    | Ethan Brown    | 2             |
| 1008    | George Clark   | 2             | 

## Solution:

```sql
USE FS;
SELECT o.OrderID, c.Name AS CustomerName, COUNT(OrderItemID) AS NumberOfItems FROM Customers c NATURAL JOIN Orders o NATURAL JOIN OrderItems i GROUP BY o.OrderID HAVING COUNT(OrderItemID) > 1 ORDER BY c.Name;
```