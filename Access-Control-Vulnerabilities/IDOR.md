# Insecure Direct Object References (IDOR) in Broken Access Control

**Insecure Direct Object References (IDOR)** is a common **Broken Access Control** vulnerability that occurs when an application **exposes access to internal objects (e.g., database records, files, or user profiles) based on user input**, **without properly verifying the user's authorization**.

---

## Scenario: IDOR Vulnerability

### Context:
A web application allows users to view their profile using a URL like: GET /user/profile?id=1234


Where `id=1234` is the ID of the currently logged-in user.

---

### Exploitation Scenario:

#### An attacker, who is a legitimate user with ID `1234`, **modifies the URL** as follows: GET /user/profile?id=1235


➡ If the server does **not validate access rights**, and returns the profile data of user `1235`, then the application is **vulnerable to IDOR**.

---

## Potential Impacts:

- **Sensitive data disclosure** (emails, phone numbers, personal or medical records).
- **Account takeover** (if the endpoint allows updating user information).
- **Unauthorized file downloads or deletions**.
- **Privilege escalation** (accessing admin-only resources).

---

## Common Endpoints Vulnerable to IDOR:

| HTTP Method | Example Endpoint                                  | Description                          |
|-------------|---------------------------------------------------|--------------------------------------|
| `GET`       | `/invoice/download?file=invoice_125.pdf`          | Downloading someone else's invoice   |
| `POST`      | `/user/update?id=5001`                            | Updating another user's data         |
| `DELETE`    | `/message/delete?id=311`                          | Deleting messages of another user    |
| `GET`       | `/admin/viewReport?reportId=77`                   | Viewing admin-only reports           |

---

## How to Prevent IDOR:

1. **Always perform authorization checks** on the server-side:
   - Ensure the current user is allowed to access or modify the requested object.

2. **Avoid predictable identifiers (e.g., sequential IDs)**:
   - Use UUIDs or opaque tokens instead of numeric IDs.

3. **Design RESTful endpoints to scope data to the current user**:
   - Use `/me/profile` instead of `/user/profile?id=...`.

4. **Never trust user input for access control**:
   - Implement a "Zero Trust" approach in API and backend development.

5. **Use built-in authorization mechanisms** in your frameworks:
   - For example, Laravel middleware, Django permissions, ASP.NET policies, etc.

---

## Summary:

**IDOR is a serious Broken Access Control flaw that is often overlooked.** It's especially dangerous when used with endpoints that allow editing, deleting, or downloading sensitive resources.

## Further Reading

- [OWASP - Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
- [PortSwigger - Access Control Vulnerabilities](https://portswigger.net/web-security/access-control)
---





