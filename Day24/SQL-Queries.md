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

###  1. Find names for departments with no employees.

Expected Output Columns:

| dname |
| ----- |

```sql
USE test; 
SELECT dname FROM dept WHERE deptno NOT IN (SELECT DISTINCT deptno FROM emp);
```


###  2. Find the department that has the most employees.

Expected Output Columns:

| deptno | employee_count | 
| ------ | -------------- |

```sql
USE test;
SELECT deptno, COUNT(empno) AS employee_count FROM emp GROUP BY deptno ORDER BY COUNT(empno) DESC LIMIT 1;
```

### 3. Find the department name where ‘JONES’ works.

Expected Output Columns:

| dname |
| ----- |

```sql
USE test;
SELECT dname FROM dept WHERE deptno IN (SELECT deptno FROM emp WHERE ename='JONES');
```

### 4. Write a SQL query to find employees whose name contains ‘A’.

Expected Output Columns:

| ename | empno | 
| ----- | ----- |

```sql
USE test;
SELECT ename, empno FROM emp WHERE ename LIKE "%A%";
```

### 5. Retrieve employees who have a commission greater than their salary.

Expected Output Columns:

| ename | empno | sal | comm | 
| ----- | ----- | --- | ---- |

```sql
USE test;
SELECT ename, empno, sal, comm FROM emp WHERE comm>sal;
```
