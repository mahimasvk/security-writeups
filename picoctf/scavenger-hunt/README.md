# PicoCTF - Scavenger Hunt (Web Exploitation)

## Challenge Link
[Scavenger Hunt CTF Link](https://play.picoctf.org/practice/challenge/161?category=1&difficulty=1&page=2)

## Overview
This is a beginner web exploration challenge that requires inspecting the website to find a hidden flag. 
The goal is to explore all page elements and clues using basic web tools like a browser inspector.


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
