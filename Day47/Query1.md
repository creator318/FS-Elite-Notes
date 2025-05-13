#SQL 

Find customers who have Gmail email addresses and have ordered food items whose names consist of exactly two words.

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

| full_name   | email                 | name            |
| ----------- | --------------------- | --------------- |
| Amit Sharma | amit.sharma@gmail.com | Chicken Biryani |
| Amit Sharma | amit.sharma@gmail.com | Gulab Jamun     |

## Solution:

```sql
USE GT;
SELECT CONCAT(c.first_name," ",c.last_name) AS full_name, c.email, f.name FROM Customers c NATURAL JOIN Orders o NATURAL JOIN FoodItems f WHERE c.email REGEXP 'gmail' AND f.name REGEXP '^([A-Za-z]+){2}$';
```
