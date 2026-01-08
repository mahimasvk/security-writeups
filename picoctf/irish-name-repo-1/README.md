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
1. Reviewed the challenge hints, which suggested that user credentials are stored in a database. This implied that the login functionality relies on a SQL query for authentication.
2. Inspected the page source and discovered a hidden input field labeled `debug` that was set to `0`. Since hidden fields are still sent to the server, the value was modified to `1` to enable debug output.
3. Submitted a test username and password to observe backend behavior. With debugging enabled, the website revealed the full SQL query used for login verification:
```sql
SELECT * FROM users
WHERE name='admin' AND password='admin';
```
4. Observing that user input was directly embedded into the SQL query without sanitization, it became clear that the application was vulnerable to SQL injection.
5. By leveraging control over the string delimiter and logical operators in the query, the authentication condition was manipulated to always evaluate as true. Commenting out the remaining portion of the query prevented syntax errors and bypassed the password check entirely.
6. As a result, authentication was successfully bypassed and access to the admin page was granted, revealing the flag.

---

## Key Concepts Explained
### Concept
Description

---

## Tools Used
- Tools used

---

## Lessons Learned
- Lessons

---

## Security Implications
What this challenge shows about security.

---

## Mitigation
- What not to do to avoid security leaks.

---

