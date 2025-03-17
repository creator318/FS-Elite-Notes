#CyberSecurity/Command-Injection #CyberSecurity/Broken-Authentication

### 1. Given the following vulnerable code, what type of attack can be performed?
```js
exec(`ping ${req.body.host}`, (error, stdout, stderr) => { ... });
```
- ❌ Cross-Site Scripting (XSS)
- ❌ SQL Injection
- ✅ Command Injection
- ❌ CSRF Attack

### 2. How can Broken Access Control be exploited?
- ❌ By logging in with the wrong password
- ❌ By making too many API requests
- ❌ By using a strong password
- ✅ By modifying JWT tokens or accessing restricted APIs

### 3. How can the following function be exploited?
```js
app.post('/track-vehicle', (req, res) => {
    const { plateNumber } = req.body;
    exec(`echo Tracking vehicle ${plateNumber}`, (error, stdout, stderr) => { ... });
});
```

- ❌ By using a VPN
- ✅ By injecting shell commands in the plateNumber field
- ❌ By sending an empty request body
- ❌ By making multiple requests at the same time

### 4. If a user inputs `ABC123 && rm -rf /`, what will happen on a Linux server?
- ❌ Nothing will happen
- ❌ The server will shut down immediately
- ✅ The entire file system could be deleted
- ❌ The vehicle tracking system will show an error

### 5. What command could an attacker enter in the `/track-vehicle` endpoint to delete files on a Windows system?
- ❌ `ABC123; rm -rf /`
- ✅  `ABC123 && del C:\Windows\System32`
- ❌ `ABC123 && shutdown -h now`
- ❌ `ABC123 && mv /etc/passwd /dev/null`

### 6. What is the best way to prevent command injection attacks?
- ❌ Use an insecure API to execute shell commands
- ✅ Use parameterized queries and sanitize input
- ❌ Use `eval()` to process user input
- ❌ Allow user input directly in system commands

### 7. What is the correct way to restrict access to admin users only?
- ❌ `if (decoded.role !== 'user') return res.status(403).json({ error: 'Forbidden' });`
- ❌ `if (decoded.id === 1) return res.status(403).json({ error: 'Forbidden' });`
- ❌ `if (!decoded.role) return res.status(403).json({ error: 'Forbidden' });`
- ✅  `if (decoded.role !== 'admin') return res.status(403).json({ error: 'Forbidden' });`

### 8. What is the impact of Broken Access Control on an application?
- ❌ The database gets automatically deleted
- ❌ Attackers can execute arbitrary commands on the server
- ✅ Unauthorized users can access restricted information or perform admin actions
- ❌ It allows Cross-Site Scripting (XSS)

### 9. What is the primary cause of command injection vulnerabilities in applications?
- ❌ Poor network security configuration
- ❌ Incorrect use of loops in JavaScript
- ✅ Lack of input validation when executing system commands
- ❌ Using HTTPS instead of HTTP

### 10. What is the safest way to execute system commands in Node.js?
- ❌ Using `exec()` with user input
- ❌ Concatenating user input into system commands
- ❌ Using `eval()`
- ✅ Using `execFile()` with sanitized input

### 11. What security flaw exists in the following `/users` endpoint?
```js
app.get('/users', (req, res) => {
    const token = req.headers.authorization;
    jwt.verify(token, SECRET_KEY, (err, decoded) => {
        db.query('SELECT id, username, role FROM users', (err, results) => {
            res.json({ users: results });
        });
    });
});
```
- ❌ It is vulnerable to SQL injection
- ✅ It does not verify the user’s role before returning data
- ❌ It does not return JSON data
- ❌ It does not store passwords securely

### 12. What would happen if an attacker modified a JWT token to escalate their privileges?
- ❌ They would get logged out
- ❌ The server would detect the modification and reject the request
- ❌ The token would expire immediately
- ✅ They could access admin-only features

### 13. Which function is the most dangerous when handling user input in Node.js?
- ❌ `parseInt()`
- ✅  `exec()`
- ❌ `JSON.stringify()`
- ❌ `console.log()`

### 14. Which of the following is an effective way to prevent Broken Access Control?
- ❌ Store JWT tokens in Local Storage without encryption
- ❌ Remove authentication from sensitive endpoints
- ❌ Allow users to modify their own JWT tokens
- ✅ Validate user roles and permissions before processing requests

### 15. Why is the following endpoint a security risk?
```js
app.get('/users', (req, res) => {
    db.query('SELECT id, username, role FROM users', (err, results) => {
        res.json({ users: results });
    });
});
```
- ❌ It allows SQL Injection
- ❌ It uses HTTPS instead of HTTP
- ❌ It is vulnerable to CSRF
- ✅ It exposes all users' details without authentication
