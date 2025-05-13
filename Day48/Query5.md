#SQL 

Display customer emails and categorize them based on the email provider ('Gmail', 'Yahoo', or 'Other').

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

| email                 | email_provider |
| --------------------- | -------------- |
| amit.sharma@gmail.com | Gmail          |
| neha.patel@yahoo.com  | Yahoo          |


## Solution:

```sql
USE GT;
SELECT email, (CASE 
	WHEN email LIKE '%gmail.com' THEN 'Gmail' 
	WHEN email LIKE '%yahoo.com' THEN 'Yahoo'
	ELSE 'Other' 
END) as email_provider FROM Customers
```