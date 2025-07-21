# Lab: Unprotected admin functionality with unpredictable URL
## Target: Access admin panel and delete "carlos"

**URL:** `https://0a7b00400335b3b1808f7b180005000e.web-security-academy.net/`
## Creds: wiener:peter

## Solution
1. Review the lab home page's source using Burp Suite or your web browser's developer tools.
2. Observe that it contains some JavaScript that discloses the URL of the admin panel.
3. Load the admin panel and delete "carlos".

## ✅ Result: screenshots/Result.png

## Screenshot
![JavaScript that discloses the URL of the admin panel](screenshots/admin-js.png)
![SResult](screenshots/Result.png)