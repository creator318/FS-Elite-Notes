#SQL

# Write the queries for the given criteria for the test database as follows:

### Tables:

| Tables_in_test |
| -------------- |
| dept           |
| emp            |
| salgrade       | 

#### Schema: dept
| Field    | Type        | Null | Key | Default | Extra |
| -------- | ----------- | ---- | --- | ------- | ----- |
| deptno   | int         | NO   | PRI | NULL    |       |
| dname    | varchar(50) | NO   |     | NULL    |       |
| location | varchar(50) | NO   |     | NULL    |       |

#### Schema: emp
| Field    | Type          | Null | Key | Default | Extra |
| -------- | ------------- | ---- | --- | ------- | ----- |
| empno    | int           | NO   | PRI | NULL    |       |
| ename    | varchar(50)   | NO   |     | NULL    |       |
| job      | varchar(50)   | NO   |     | NULL    |       |
| mgr      | int           | YES  |     | NULL    |       |
| hiredate | date          | YES  |     | NULL    |       |
| sal      | decimal(10,2) | YES  |     | NULL    |       |
| comm     | decimal(10,2) | YES  |     | NULL    |       |
| deptno   | int           | YES  | MUL | NULL    |       | 

#### Schema: salgrade
| Field | Type          | Null | Key | Default | Extra |
| ----- | ------------- | ---- | --- | ------- | ----- |
| grade | int           | NO   | PRI | NULL    |       |
| losal | decimal(10,2) | YES  |     | NULL    |       |
| hisal | decimal(10,2) | YES  |     | NULL    |       |

###  1. Retrieve the names and salaries of employees who have the job title "SALESMAN" and earn more than 1500.

Expected Output Columns:

| ename | sal | 
| ----- | --- |

```sql
USE test; 
SELECT ename, sal FROM emp WHERE job='SALESMAN' AND sal>1500;
```


###  2. List all employees whose job title is either "MANAGER" or "ANALYST".

Expected Output Columns:

| ename | job | 
| ----- | --- |


```sql
USE test;
SELECT ename, job FROM emp WHERE job IN ('MANAGER', 'ANALYST'); 
```

### 3. Get all the details of employees who do not belong to department 20.

```sql
USE test;
SELECT * FROM emp WHERE deptno!=20;
```

### 4. Get the employee names and their commission details where the job is "CLERK" or the commission is more than 1000.

Expected Output Columns:

| ename | job | comm | 
| ----- | --- | ---- |

```sql
USE test;
SELECT ename, job, comm FROM emp WHERE job="CLERK" OR comm>1000;
```

### 5. Get the list of employees who do not have the job title "CLERK" or "SALESMAN".

Expected Output Columns:

| ename | job |
| ----- | --- |

```sql
USE test;
SELECT ename, job FROM emp WHERE job NOT IN ('CLERK', 'SALESMAN');
```

### 6. Retrieve the names and salaries of employees who earn between 1000 and 3000 (inclusive).

Expected Output Columns:

| ename | sal | 
| ----- | --- |

```sql
USE test;
SELECT ename, sal FROM emp where sal BETWEEN 1000 AND 3000;
```

### 7. Retrieve employees who were hired between the years 1995 and 1999.

Expected Output Columns:

| ename | hiredate | 
| ----- | -------- |

```sql
USE test;
SELECT ename, hiredate FROM emp WHERE YEAR(hiredate) BETWEEN 1995 AND 1999;
```

### 8. Retrieve employees who earn a commission between 300 and 1000.

Expected Output Columns:

| ename | comm | 
| ----- | ---- |

```sql
USE test;
SELECT ename, comm FROM emp WHERE comm BETWEEN 300 AND 1000;
```

### 9. Retrieve employees who are part of department numbers between 20 and 40.

Expected Output Columns:

| ename | deptno |
| ----- | ------ |

```sql
USE test;
SELECT ename, deptno FROM emp WHERE deptno BETWEEN 20 AND 40;
```

### 10. List all salary grades where the grade is between 2 and 4.

Expected Output Columns:

| grade | losal | hisal |
| ----- | ----- | ----- |

```sql
USE test;
SELECT grade, losal, hisal FROM salgrade WHERE grade BETWEEN 2 AND 4;
```

### 11. Get employees who are not managers and have a salary below 2000.

Expected Output Columns:

| ename | job | sal |
| ----- | --- | --- |

```sql
USE test;
SELECT ename, job, sal FROM emp WHERE job!="MANAGER" AND sal<2000;
```

### 12. Retrieve employees who do not report to any manager.

Expected Output Columns:

| ename | job | mgr | 
| ----- | --- | --- |

```sql
USE test;
SELECT ename, job, mgr FROM emp WHERE mgr IS NULL;
```

### 13. Get clerks or analysts hired between 1996 and 2000

Expected Output Columns:

| ID  | Name | StartDate | 
| --- | ---- | --------- |

```sql
USE test;
SELECT empno AS ID, ename AS Name, hiredate AS StartDate FROM emp WHERE job IN ('CLERK', 'ANALYST') AND YEAR(hiredate) BETWEEN 1996 AND 2000;
```

### 14. List employees with mgr 7788 or 7566 and sal < 3000

Expected Output Columns:

| Employee | Boss | Salary | 
| -------- | ---- | ------ |

```sql
USE test;
SELECT ename AS Employee, mgr AS Boss, sal AS Salary FROM emp WHERE mgr IN (7788, 7566) AND sal<3000;
```

### 15. Retrieve employees with 'ar' anywhere in their name

Expected Output Columns:

| empno | ename | job | mgr | hiredate | sal | comm | deptno | 
| ----- | ----- | --- | --- | -------- | --- | ---- | ------ |

```sql
USE test;
SELECT empno, ename, job, mgr, hiredate, sal, comm, deptno FROM emp WHERE ename LIKE '%ar%';
```

### 16. Retrieve employees with exactly 5-letter names starting with 'K'.

Expected Output Columns:

| empno | ename | job | mgr | hiredate | sal | comm | deptno |
| ----- | ----- | --- | --- | -------- | --- | ---- | ------ |

```sql
USE test;
SELECT empno, ename, job, mgr, hiredate, sal, comm, deptno FROM emp WHERE ename LIKE 'k____';
```

### 17. Retrieve employees whose names do not end with 'n'.

Expected Output Columns:

| empno | ename | job | mgr | hiredate | sal | comm | deptno | 
| ----- | ----- | --- | --- | -------- | --- | ---- | ------ |

```sql
USE test;
SELECT empno, ename, job, mgr, hiredate, sal, comm, deptno FROM emp WHERE ename NOT LIKE '%n';
```

### 18. Retrieve which employees have names starting with 'C' or 'S'.

Expected Output Columns:

| empno | ename | job | mgr | hiredate | sal | comm | deptno | 
| ----- | ----- | --- | --- | -------- | --- | ---- | ------ |

```sql
USE test;
SELECT empno, ename, job, mgr, hiredate, sal, comm, deptno FROM emp WHERE ename LIKE 'c%' OR ename LIKE 's%';
```

### 19. Perform Sum of salaries and commissions by dept 30 

Expected Output Columns:

| TotalPay | TotalBonus | 
| -------- | ---------- |

```sql
USE test;
SELECT SUM(sal) AS TotalPay, SUM(comm) AS TotalBonus FROM emp WHERE deptno=30;
```

### 20. Get max and min salaries in dept 20 or 40

Expected Output Columns:

| HighestPay | LowestPay | 
| ---------- | --------- |

```sql
USE test;
SELECT MAX(sal) AS HighestPay, MIN(sal) AS LowestPay FROM emp WHERE deptno IN (20, 40);
```