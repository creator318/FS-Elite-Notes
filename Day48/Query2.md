#SQL 

List food item names, their prices, and a status indicating whether the item is "Expensive" (price greater than ₹150) or "Affordable" (₹150 or less).

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

| food_item            | price  | price_category |
| -------------------- | ------ | -------------- |
| Paneer Butter Masala | 250.00 | Expensive      |
| Chicken Biryani      | 300.00 | Expensive      |
| Masala Dosa          | 120.00 | Affordable     |

## Solution:

```sql
USE GT;
SELECT name AS food_item, price, (CASE 
	WHEN price > 150 THEN 'Expensive' 
	ELSE 'Affordable' 
END) AS price_category FROM FoodItems;
```