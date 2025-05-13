#SQL 

Show food items with the total ordered quantity and classify them as 'Bestseller' if total quantity ≥5, otherwise 'Regular'.

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

| name            | total_quantity | popularity_status |
| --------------- | -------------- | ----------------- |
| Chicken Biryani | 3              | Regular           |
| Masala Dosa     | 5              | Bestseller        |


## Solution:

```sql
USE GT;
SELECT name, SUM(quantity) AS total_quantity, (CASE 
	WHEN total_quantity >= 5 THEN 'Bestseller' 
	ELSE 'Regular' 
END) AS popularity_status FROM Orders o NATURAL JOIN FoodItems f GROUP BY o.food_id
```