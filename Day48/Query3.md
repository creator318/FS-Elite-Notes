#SQL 

Display all orders along with customer full name, order status, and a custom message based on the status.

- Delivered -> Order Completed Successfully
- Preparing -> Order In Progress
- Pending -> Awaiting Confirmation
- Others -> Order Cancelled

Customers Table
==================
```
customer_id INT AUTO_INCREMENT PRIMARY KEY,
first_name VARCHAR(100) NOT NULL,
last_name VARCHAR(100) NOT NULL,
email VARCHAR(150) UNIQUE NOT NULL,
phone VARCHAR(15),
address TEXT
```

FoodItems Table
==================
```
food_id INT AUTO_INCREMENT PRIMARY KEY,
name VARCHAR(100) NOT NULL,
description TEXT,
price DECIMAL(8,2) NOT NULL,
category VARCHAR(50),
availability BOOLEAN DEFAULT TRUE
```

Orders Table
================
```
order_id INT AUTO_INCREMENT PRIMARY KEY,
customer_id INT NOT NULL,
food_id INT NOT NULL,
quantity INT NOT NULL,
order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
status ENUM('Pending', 'Preparing', 'Delivered', 'Cancelled') DEFAULT 'Pending',
total_amount DECIMAL(10,2) NOT NULL,
FOREIGN KEY (customer_id) REFERENCES Customers(customer_id),
FOREIGN KEY (food_id) REFERENCES FoodItems(food_id)
```

Sample Output:
==============

| order_id | customer_name | status    | order_message                |
| -------- | ------------- | --------- | ---------------------------- |
| 1        | Amit Sharma   | Delivered | Order Completed Successfully |
| 4        | Priya Singh   | Preparing | Order In Progress            |
| 19       | Priya Singh   | Pending   | Awaiting Confirmation        |

## Solution:

```sql
USE GT;
SELECT o.order_id, CONCAT(c.first_name, ' ', c.last_name) AS customer_name, o.status, (CASE 
	WHEN o.status = 'Delivered' THEN 'Order Completed Successfully'
	WHEN o.status = 'Preparing' THEN 'Order In Progress'
	WHEN o.status = 'Pending' THEN 'Awaiting Confirmation'
	ELSE 'Order Cancelled'
END) AS order_message FROM Orders o NATURAL JOIN Customers c;
```