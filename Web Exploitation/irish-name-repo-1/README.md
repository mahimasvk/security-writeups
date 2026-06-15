# PicoCTF - Irish Name Repo 1 (Web Exploitation)

--- 

## Challenge Link
[Irish Name Repo 1 CTF Link](https://play.picoctf.org/practice/challenge/80?category=1&page=1&search=irish&solved=0)

## Overview
This is an intermediate web exploitation challenge focused on logic and input validation.
The goal is to bypass an admin login page by identifying and exploiting insecure database queries
used to verify user credentials.

---

## Attack / Solution
1. Reviewed the challenge's hints, which suggested that user info is stored in a database.
2. This implies that the login functionality relies on a SQL query for authentication.
3. I inspected the page source and discovered a hidden input field labeled `debug` that was set to `0`.
4. This implies that a debug test was accidentally leaked and set to false to be hidden.
5. Since hidden fields are still sent to the server, I modified the value to `1` (true) to show the debug output.
6. I submitted 'xyz' for both the username and password to observe backend behavior.
7. With the debugging test enabled, the website revealed the full SQL query used for login verification:
```sql
username: xyz
password: xyz
SQL query: SELECT * FROM users WHERE name='xyz' AND password='xyz'
```
4. This shows that user input is directly embedded into the SQL query, so the site would be vulnerable to SQL injection.
5. To test, I changed both inputs to close the first single quote, accept a true statement, and then close the second single quote:
```sql
username: ' OR true OR '
password: ' OR true OR '
SQL query: SELECT * FROM users WHERE name='' OR true OR '' AND password='' OR true OR ''
```
6. As a result, authentication was successfully bypassed and access to the admin page was granted, revealing the flag.

---

## Key Concepts Explained
### SQL Injection
SQL injection occurs when user input is inserted directly into a database query without proper validation.
This allows attackers to modify the structure or logic of the query itself rather than supplying expected data.

### Authentication Logic
Many login systems rely solely on whether a database query returns any rows.
If the query logic is altered to always return a result, authentication can be bypassed entirely.

### Debug Information Disclosure
Exposing debugging features in production environments can reveal sensitive internal logic,
such as database queries, which greatly lowers the barrier for exploitation.

---

## Tools Used
- Browser developer tools
- Source Inspection
- SQL Injection

---

## Lessons Learned
- Authentication mechanisms should never rely on unsanitized user input.
- Debug features must be removed or disabled before deployment.
- Understanding how backend queries are constructed is critical for identifying vulnerabilities.
- SQL injection is fundamentally a logic vulnerability, not a brute force attack.

---

## Security Implications
This challenge shows how improper input handling and debugging disclosure can compromise authentication systems.
Even simple login forms can become critical vulnerabilities if database queries are constructed insecurely.

---

## Mitigation
- Use prepared statements or parameterized queries.
- Never expose debugging information to users.
- Validate and sanitize all user input.
- Implement additional authentication safeguards beyond a single database check.

---

