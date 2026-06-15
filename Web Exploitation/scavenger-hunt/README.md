# PicoCTF - Scavenger Hunt (Web Exploitation)

--- 

## Challenge Link
[Scavenger Hunt CTF Link](https://play.picoctf.org/practice/challenge/161?category=1&difficulty=1&page=2)

## Overview
This is a beginner web exploration challenge that requires inspecting the website to find a hidden flag. 
The goal is to explore all page elements and clues using basic web tools like a browser inspector.

---

## Attack / Solution
1. Viewed the page source to inspect content.
2. Found the first part of the flag directly in the main page source.
3. Identified linked CSS and JavaScript files and reviewed them for comments or hidden clues.
5. Found the second part within a comment in a linked CSS file.
6. While inspecting the JavaScript file, I noticed a comment about preventing Google from indexing the site.
7. This suggested checking the website's `robots.txt` file to see what the site didn't want indexed, and the third part was found.
9. In that file, there was a comment referencing that this website was built on an Apache server.
10. This indicated checking potentially public Apache configuration files by running `.htaccess`, where the fourth part was.
11. A final hint in the comments mentioned using a MacBook for its storage capabilities.
12. Assuming this website was uploaded from a Mac, there was a possibility that the DS_Store file was intentionally uploaded.
13. In the `.DS_Store` file, I found the final part of the flag.

---

## Key Concepts Explained
### Apache
Apache is a widely used open-source web server that handles HTTP requests.
It allows configuration through files like `.htaccess`.
If these configuration files are accidentally exposed, they can leak sensitive information.

### robots.txt
`robots.txt` is a file used to instruct search engines which parts of a website should not be indexed.
It does NOT provide security. Any user can manually access the file and view its contents.

### .htaccess
`.htaccess` is an Apache configuration file used to control directory behavior.
If publicly accessible, it can reveal server rules, hidden paths, or sensitive data.

### .DS_Store
`.DS_Store` is a macOS system file that stores folder metadata.
If uploaded to a web server, it can expose internal directory structure and filenames.

---

## Tools Used
- Web browser developer tools
   Page Source
- Manual URL manipulation
- Inference from developer comments

---

## Lessons Learned
- Client-side files often contain more information than intended.
- `robots.txt` should never be relied on as a security mechanism.
- Server configuration files must be properly secured.
- Operating system artifacts can introduce serious information disclosure vulnerabilities.

---

## Security Implications
This challenge demonstrates how poor file safety and misconfigured servers
can lead to unintended information disclosure without exploiting complex vulnerabilities.

---

## Mitigation
- Do not store secrets or hints in client-side comments.
- Restrict access to configuration files like `.htaccess`.
- Never upload `.DS_Store` or OS-generated metadata files.

---
