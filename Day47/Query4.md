#SQL

Find the names and categories of food items from the 'Main Course' category ordered by customers whose last names end with either 'a' or 'i'.

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

| name                 | category    |
| -------------------- | ----------- |
| Paneer Butter Masala | Main Course |
| Chicken Biryani      | Main Course |



## Solution:

```sql
USE GT;
SELECT DISTINCT f.name, f.category FROM FoodItems f NATURAL JOIN Orders o NATURAL JOIN Customers c WHERE c.last_name REGEXP "(a|i)$" AND f.category = "Main Course";
```