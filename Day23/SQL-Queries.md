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

###  1. Find employees who earn more than the average salary of all employees.

Expected Output Columns:

| ename | sal | 
| ----- | --- |

```sql
USE test; 
SELECT ename, sal FROM emp WHERE sal>(SELECT AVG(sal) FROM emp);
```


###  2. Find the name and salary of the highest-paid employee.

Expected Output Columns:

| ename | sal | 
| ----- | --- |

```sql
USE test;
SELECT ename, sal FROM emp ORDER BY sal DESC LIMIT 1;
```

### 3. Find employees who earn more than the highest-paid employee in department 10.

Expected Output Columns:

| ename | sal | deptno | 
| ----- | --- | ------ |

```sql
USE test;
SELECT ename, sal, deptno FROM emp WHERE sal>(SELECT MAX(sal) FROM emp WHERE deptno=10);
```

### 4. Find employees whose salary is higher than the salary of ‘SCOTT’.

Expected Output Columns:

| ename | sal |
| ----- | --- |

```sql
USE test;
SELECT ename, sal FROM emp WHERE sal>(SELECT sal FROM emp WHERE ename='SCOTT');
```

### 5. Find employee who have the same job title as 'FORD'.

Expected Output Columns:

| ename | job |
| ----- | --- |

```sql
USE test;
SELECT ename, job FROM emp WHERE job IN (SELECT job FROM emp WHERE ename='FORD');
```

### 6. Find departments that have at least one employee earning more than 3000.

Expected Output Columns:

| deptno | dname | 
| ------ | ----- |

```sql
USE test;
SELECT deptno, dname FROM dept WHERE deptno IN (SELECT deptno FROM emp WHERE sal > 3000); 
```

### 7. Find employees who were hired before all employees in department 30.

Expected Output Columns:

| ename | hiredate | 
| ----- | -------- |

```sql
USE test;
SELECT ename, hiredate FROM emp WHERE hiredate < ALL(SELECT hiredate FROM emp WHERE deptno=30); 
```

### 8. Find employees who belong to departments located in 'Dallas'.

Expected Output Columns:

| ename | deptno | 
| ----- | ------ |

```sql
USE test;
SELECT ename, deptno FROM emp WHERE deptno IN (SELECT deptno FROM dept WHERE location='Dallas');
```

### 9. Find the second highest salary from employees.

Expected Output Columns:

| second_highest_salary | 
| --------------------- | 

```sql
USE test;
SELECT sal as second_highest_salary FROM emp ORDER BY sal DESC LIMIT 1 OFFSET 1;
```

### 10. Find employees who have the same manager as ‘BLAKE’.

Expected Output Columns:

| ename | mgr |
| ----- | --- |

```sql
USE test;
SELECT ename, mgr FROM emp WHERE mgr=(SELECT mgr FROM emp WHERE ename='BLAKE') AND ename!='BLAKE';
```
