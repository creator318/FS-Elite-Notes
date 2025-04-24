#SQL 

# Write the queries for the given criteria for the test database as follows:

### Tables:

| Tables_in_test |
| -------------- |
| dept           |
| emp            |
| salgrade       | 

#### Schema: dept
| Field    | Type         | Null | Key | Default | Extra |
| -------- | ------------ | ---- | --- | ------- | ----- |
| deptno   | int          | NO   | PRI | NULL    |       |
| dname    | varchar (50) | NO   |     | NULL    |       |
| location | varchar (50) | NO   |     | NULL    |       |

#### Schema: emp
| Field    | Type           | Null | Key | Default | Extra |
| -------- | -------------- | ---- | --- | ------- | ----- |
| empno    | int            | NO   | PRI | NULL    |       |
| ename    | varchar (50)   | NO   |     | NULL    |       |
| job      | varchar (50)   | NO   |     | NULL    |       |
| mgr      | int            | YES  |     | NULL    |       |
| hiredate | date           | YES  |     | NULL    |       |
| sal      | decimal (10,2) | YES  |     | NULL    |       |
| comm     | decimal (10,2) | YES  |     | NULL    |       |
| deptno   | int            | YES  | MUL | NULL    |       |

#### Schema: salgrade
| Field | Type           | Null | Key | Default | Extra |
| ----- | -------------- | ---- | --- | ------- | ----- |
| grade | int            | NO   | PRI | NULL    |       |
| losal | decimal (10,2) | YES  |     | NULL    |       |
| hisal | decimal (10,2) | YES  |     | NULL    |       |

### 1. Write a SQL query to find all employees who do not receive a commission.

Expected Output Columns:

| ename | empno | comm |
| ----- | ----- | ---- |

```sql
USE test;
SELECT ename, empno, comm FROM emp WHERE comm=0.00 OR comm IS NULL;
```

###  2. Write a SQL query to count the number of employees who have a manager.

Expected Output Columns:

| employees_with_manager |
| ---------------------- |

```sql
USE test;
SELECT COUNT(empno) AS employees_with_manager FROM emp WHERE mgr IS NOT NULL;
```

### 3. Write a SQL query to find the difference between the highest and second highest salary.

Expected Outout Columns:

| salary_difference |
| ----------------- |

```sql
USE test;
SELECT MAX(sal) - (SELECT sal FROM emp ORDER BY sal DESC LIMIT 1 OFFSET 1) AS salary_difference FROM emp;
```

### 4. Write a SQL query to calculate the total salary and total commission for all employees.

Expected Output Columns:

| Total Salary | Total Commission |
| ------------ | ---------------- |

```sql
USE test;
SELECT SUM(sal) AS "Total Salary", SUM(comm) AS "Total Commission" FROM emp;
```

### 5. Write a SQL query to calculate the average salary and average commission of employees.

Expected Output Columns:

| Average Salary | Average Commission |
| -------------- | ------------------ |

```sql
USE test;
SELECT AVG(sal) AS "Average Salary", AVG(comm) as "Average Commission" FROM emp;
```

### 6. Write a SQL query to calculate the average salary of employees with a commission.

Expected Output Columns:

| avg_salary_with_comm |
| -------------------- |

```sql
USE test;
SELECT AVG(sal) AS avg_salary_with_comm FROM emp WHERE comm IS NOT NULL;
```

### 7. Write a SQL query to determine the minimum commission value, excluding NULLs.

Expected Output Columns:

| min_commission |
| -------------- |

```sql
USE test;
SELECT MIN(comm) AS min_commission FROM emp WHERE comm IS NOT NULL;
```

### 8. Write a SQL query to find the total commission paid to employees hired after 1995.

Expected Output Columns:

| total_comm_post_1995 |
| -------------------- |

```sql
USE test;
SELECT SUM(comm) AS total_comm_post_1995 FROM emp WHERE YEAR(hiredate) > 1995;
```

### 9. Write a SQL query to find the maximum hire date (latest hire) in the emp table.

Expected Output Columns:

| latest_hire |
| ----------- |

```sql
USE test;
SELECT MAX(hiredate) AS latest_hire FROM emp;
```

### 10. Write a SQL query to find the average commission for salesmen, excluding NULLs.

Expected Output Columns:

| avg_salesman_comm |
| ----------------- |

```sql
USE test;
SELECT AVG(comm) AS avg_salesman_comm FROM emp WHERE job="Salesman" AND job IS NOT NULL;
```

### 11. Write a SQL query to determine the minimum salary for employees hired in the 1990 s.

Expected Output Columns:

| min_salary_90 s |
| --------------- |

```sql
USE test;
SELECT MIN(sal) AS min_salary_90s FROM emp WHERE YEAR(hiredate) BETWEEN 1990 AND 1999;
```

### 12. Write a SQL query to find the total salary of employees whose names start with 'K'.

Expected Output Columns:

| total_salary_k |
| -------------- |

```sql
USE test;
SELECT SUM(sal) AS total_salary_k FROM emp WHERE ename LIKE 'K%';
```

### 13. Write a SQL query to count the number of employees hired in each year.

Expected Output Columns:

| hire_year | hires_per_year |
| --------- | -------------- |

```sql
USE test;
SELECT YEAR(hiredate) AS hire_year, COUNT(eno) AS hires_per_year FROM emp GROUP BY YEAR(hiredate);
```

### 14. Write a SQL query to sum the commissions for employees with salaries below 1500.

Expected Output Columns:

| total_comm_low_salary |
| --------------------- |

```sql
USE test;
SELECT SUM(comm) AS total_comm_low_salary FROM emp WHERE sal<1500;
```

### 15. List employees who do not receive a commission but earn more than 2500.

Expected Output Columns:

| ename | sal | comm |
| ----- | --- | ---- |

```sql
USE test;
SELECT ename, sal, comm FROM emp WHERE comm IS NULL AND sal>2500;
```
