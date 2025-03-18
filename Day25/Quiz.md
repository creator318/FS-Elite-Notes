#DBMS  #DSA

### 1. What is the maximum number of nodes in a binary tree of height `h` (where height is counted as the number of edges from root to the deepest node)?
- ✅ (2^{h+1} - 1)
- ❌ (h log h)
- ❌ (2^h - 1)
- ❌ (h^2)

### 2. What is the output of the following function when applied to an undirected graph represented as an adjacency list?
```
Function BFS(Node start):
    Queue Q
    Add start to Q
    While Q is not empty:
        Node u = Q.dequeue()
        print u
        For each neighbor v of u:
            If v is not visited:
                Mark v as visited
                Add v to Q
```
- ❌ Depth-First Traversal
- ✅ Breadth First Traversal
- ❌ Finding the minimum spanning tree
- ❌ Detection of cycles

### 3. Which of the following SQL statements is used to remove an entire table including its structure?
-  ❌ `REMOVE TABLE Employees;`
-  ✅ `DROP TABLE Employees;`
- ❌ `TRUNCATE TABLE Employees;`
- ❌ `DELETE TABLE Employees;`

### 4. Which of the following SQL commands can be used to modify the structure of an existing table?
-  ✅ `ALTER`
-  ❌ `MODIFY`
-  ❌ `UPDATE`
-  ❌ `CHANGE`

### 5. What will happen if we execute the following command?
```
TRUNCATE TABLE Orders;
```
- ❌ Returns an error if there are foreign key constraints.
- ❌ Deletes selected rows only.
- ✅ Deletes all rows but retains the table structure.
- ❌ Deletes all rows and removes the table structure.

### 6. Which SQL command is used to modify existing data in a table?
- ❌ `MODIFY`
- ✅ `UPDATE`
- ❌ `INSERT`
- ❌ `ALTER`

### 7. Consider the following SQL query:
```
UPDATE Employees 
SET Salary = Salary + 5000
WHERE Department = 'HR';
```
What does this query do?
- ✅ Increases salary of only HR department employees by 5000.
- ❌ Increases all employees’ salary by 5000.
- ❌ Decreases salary of HR department employees by 5000.
- ❌ Throws an error due to the `WHERE` clause.

### 8. What will happen if you execute the following SQL statement?
```
INSERT INTO Students (ID, Name) VALUES (101, 'John');
INSERT INTO Students (ID, Name) VALUES (101, 'Mike');
```
- ❌ The second statement overwrites the first one.
- ❌ Error due to missing `VALUES` keyword.
- ❌ Both rows will be inserted successfully.
- ✅ Only the first row is inserted; the second one causes a Primary Key violation.

### 9. Which SQL statement is used to give a user access to a database?
- ❌ `ALTER`
- ❌  `REVOKE`
- ✅ `GRANT`
- ❌ `ACCESS`

### 10. What will be the result of the following SQL statement?
```
REVOKE INSERT, UPDATE ON Employees FROM user1;
```
- ❌ Nothing happens.
- ✅ `user1` loses INSERT and UPDATE privileges on `Employees`.
- ❌ `user1` loses SELECT privilege on `Employees`.
- ❌ `user1` loses all privileges on `Employees`.

### 11. Which SQL command is used to permanently save a transaction?
- ❌ `SAVEPOINT`
- ❌ `UPDATE`
- ✅ `COMMIT`
- ❌ `ROLLBACK`

### 12. Consider the following pseudo-code for a function `func(Node root)` applied to a binary tree. What does it compute?
```
Function func(Node root):
    if root is NULL:
        return 0
    return 1 + func(root.left) + func(root.right)
```
- ❌ Height of the tree
- ✅ Number of nodes in the tree
- ❌ Maximum depth of the tree
- ❌ Sum of all node values

### 13. Consider the following SQL sequence:
```
BEGIN;
UPDATE Employees SET Salary = Salary + 5000 WHERE Department = 'IT';
ROLLBACK;
```
- ❌ An error occurs because `ROLLBACK` cannot undo an `UPDATE`.
- ❌ The salaries of IT employees will increase by 5000.
- ❌ Only half the rows get updated.
- ✅ No change will happen in the Employees table.

### 14. Which of the following is always true for a full binary tree with `n` nodes?
- ❌ The tree is always balanced
- ✅ Every node has either 0 or 2 children
- ❌ The height of the tree is always `log n`
- ❌ Every level is completely filled

### 15. Given a BST, which of the following elements will always be found in the left subtree of a node with value `x`?
- ❌ All elements in the tree
- ❌ Elements equal to `x`
- ❌ Elements greater than `x`
- ✅ Elements less than `x`

### 16. What is the output of the following function when applied to a BST?
```
Function findMin(Node root):
    if root is NULL:
        return NULL
    if root.left is NULL:
        return root.data
    return findMin(root.left)
```
- ❌ The height of the BST
- ✅ The minimum value in the BST
- ❌ The sum of all nodes
- ❌ The maximum value in the BST

### 17. What is the worst-case time complexity of deleting a node in an unbalanced BST with `n` nodes?
- ❌ O(log n)
- ✅ O (n)
- ❌ O(n log n)
- ❌ O(1)

### 18. Which of the following statements is true for Dijkstra’s Algorithm?
- ❌ It guarantees the shortest path in all cases
- ✅ It works only for graphs with non-negative weights
- ❌ It finds the shortest path between all pairs of nodes
- ❌ It works correctly with negative-weight cycles

### 19. What is the time complexity of Depth-First Search (DFS) on a graph with `V` vertices and `E` edges using an adjacency matrix?
- ❌ O(V)
- ❌ O(E log V)
- ❌ O(V + E)
- ✅ O(V²)

### 20. Which traversal method should be used to determine if a directed graph contains a cycle?
- ❌ Dijkstra’s Algorithm
- ❌ Kruskal’s Algorithm
- ❌ Breadth-First Search (BFS)
- ✅ Depth-First Search (DFS) with recursion stack