#SQL

List email addresses of customers along with the names of food items they've  ordered, where the food item's name contains exactly two consecutive vowels.

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

| email                 | food_item   |
| --------------------- | ----------- |
| amit.sharma@gmail.com | Butter Naan |
| arjun.gupta@gmail.com | Butter Naan |


## Solution:

```sql
USE GT;
SELECT c.email, f.name AS food_item FROM Customers c NATURAL JOIN Orders o NATURAL JOIN FoodItems f WHERE LOWER(f.name) REGEXP "[aeiou]{2}";
```