#CyberSecurity/SQL-Injections #CyberSecurity/XSS

### 1. What is the main difference between SQL Injection and XSS?
- ✅ SQL Injection targets databases, while XSS targets browsers
- ❌ Both are prevented using the same techniques
- ❌ XSS can modify SQL databases
- ❌ SQL Injection is always more dangerous than XSS

### 2. How can a developer protect against XSS attacks in a MERN stack app?
- ✅ Sanitize user input using DOMPurify or server-side escaping
- ❌ Allow script execution inside innerHTML
- ❌ Use inline JavaScript to filter malicious code
- ❌ Use dangerouslySetInnerHTML without sanitization

### 3. What is the most common way to fix XSS vulnerabilities?
- ❌ Using JavaScript's eval() function
- ❌ Storing scripts in the database
- ✅ Using DOMPurify to sanitize user input
- ❌ Allowing only admin users to enter scripts

### 4. What is SQL Injection?
- ❌ A method to securely access a database
- ❌ A tool to optimize database performance
- ❌ A way to encrypt SQL queries
- ✅ A technique used to bypass authentication and manipulate database queries

### 5. Which of the following XSS payloads is most likely to bypass filtering?
- ❌  `<script>alert('XSS')</script>`
- ❌  `<img src=x onerror=alert('XSS')>`
- ✅ All of the above
- ❌  `<iframe src=javascript:alert('XSS')>`

### 6. Which of the following is NOT a way to prevent SQL Injection?
- ❌ Using ORMs like Sequelize or Mongoose
- ❌ Escaping user input before using it in a query
- ✅ Using DOMPurify for sanitization
- ❌ Using Prepared Statements

### 7. Which of the following SQL queries is vulnerable to SQL Injection?
- ❌  `PREPARE stmt FROM 'SELECT * FROM users WHERE username = ? AND password = ?';`
- ✅  `SELECT * FROM users WHERE username = '" + user_input + "' AND password = '" + pass_input + "';`
- ❌  `SELECT * FROM users WHERE username = ? AND password = ?;`
- ❌  `SELECT * FROM users WHERE username = 'admin' AND password = 'admin';`

### 8. What is the best way to protect a Node.js MySQL database from SQL Injection?
- ✅ Use prepared statements and parameterized queries
- ❌ Validate user input with client-side JavaScript only
- ❌ Store passwords in plain text for easy authentication
- ❌ Use eval() to sanitize user input

### 9. Which of the following best describes Stored XSS?
- ❌ A script is executed immediately when injected
- ❌ A script is embedded in a URL and executed when the victim clicks the link
- ❌ XSS that only works on outdated browsers
- ✅ The malicious script is stored in the database and executed when loaded by a user

### 10. What does `--` in an SQL Injection attack do?
- ❌ Increases query execution speed
- ✅ Comments out the rest of the SQL query
- ❌ Adds an additional condition to the query
- ❌ Encrypts user input

### 11. Which payload can be used to bypass authentication in an SQL injection attack?
- ❌  `' OR username='admin' AND password='admin';`
- ✅  `' OR 1=1 --`
- ❌  `'; SELECT * FROM passwords;`
- ❌  `' AND DROP TABLE users;`

### 12. Why does this React component not execute XSS payloads?
- ❌ dangerouslySetInnerHTML is required to execute scripts
- ❌ The browser blocks all inline scripts by default
- ✅ React automatically escapes input to prevent script execution
- ❌ The comment data is stored in a secure database

### 13. How does the fixed version of the XSS Demo prevent XSS?
- ❌ By encoding all data before storing it
- ✅ By using DOMPurify to sanitize both input and output
- ❌ By only allowing administrators to post comments
- ❌ By blocking all comments containing `<script>`

### 14. What does the dangerouslySetInnerHTML property in React do?
- ❌ Prevents all forms of user input
- ❌ Encrypts JavaScript code
- ✅ Allows raw HTML to be inserted into the page
- ❌ Blocks XSS automatically

### 15. What additional security measures can prevent XSS attacks?
- ✅ All of the above
- ❌ Sanitizing user input before storing it
- ❌ Content Security Policy (CSP)
- ❌ Escaping special characters (<, >)
