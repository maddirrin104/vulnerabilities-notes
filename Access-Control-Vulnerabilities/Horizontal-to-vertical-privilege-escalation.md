# Horizontal to Vertical Privilege Escalation in Broken Access Control (BAC)

## Scenario Overview

In the context of **Broken Access Control (BAC)**, an attacker can sometimes escalate privileges **horizontally** (accessing resources of another user with the same role), and then **vertically** (gaining administrative or elevated privileges). This chained attack can be especially dangerous when misconfigurations allow access to sensitive functionalities.

---

## From Horizontal to Vertical Privilege Escalation

### 1. **Horizontal Privilege Escalation**
- An attacker is a legitimate user (e.g., User A).
- They tamper with URL parameters to access the account of another user (e.g., User B).
- Example: https://insecure-website.com/myaccount?id=456

If the application does not verify that the logged-in user owns the account with `id=456`, the attacker may view or modify another user's data.

### 2. **Targeting a Privileged User**
- Suppose the `id=456` belongs to an **administrator**.
- The attacker gains access to the admin's account page.
- This page may expose:
- Admin credentials or hashed passwords
- Options to **change the admin's password**
- Access to **privileged administrative functions**

### 3. **Vertical Privilege Escalation**
- By leveraging access to the admin account page, the attacker escalates from a normal user to an admin.
- For example:
- If the page allows password reset or email change, the attacker takes control of the admin account.
- If sensitive tokens, API keys, or system configurations are exposed, the attacker can further compromise the system.

---

## Why This Happens
This attack is possible due to **insufficient access control checks**. The application relies solely on user-supplied data (like `id` in the URL) without verifying the user's actual permissions on the server side.

---

##  Mitigation Strategies
- Enforce **role-based access control (RBAC)** on the server.
- Validate that the authenticated user is authorized to access the requested resource.
- Never trust data from the client side (including query parameters, headers, cookies).
- Use **non-predictable identifiers** like UUIDs instead of sequential IDs.

---

##  Summary

A horizontal privilege escalation vulnerability can often be escalated further into a vertical one by targeting higher-privileged accounts. Developers must implement robust access control mechanisms and never rely solely on client-side input to enforce access policies.

## Further Reading

- [OWASP - Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
- [PortSwigger - Access Control Vulnerabilities](https://portswigger.net/web-security/access-control)


