# Horizontal Privilege Escalation in BAC (Broken Access Control)

Horizontal privilege escalation occurs when **a user gains access to resources or functionalities of another user with the same privilege level**, instead of being restricted to their own.

> Example: An employee can view the personal records of another employee in a company portal.

---

## Scenario Example

Suppose a user can access their account page using the following URL: https://insecure-website.com/myaccount?id=123

If the user modifies the `id` parameter to another user’s ID, such as: https://insecure-website.com/myaccount?id=124

And the page **displays the data for user ID 124 without access checks**, then the application is vulnerable to **horizontal privilege escalation**.

---

## Underlying Vulnerability: IDOR

This is a classic case of **Insecure Direct Object Reference (IDOR)** — where the application uses **user-controlled input** to directly access resources, without properly validating whether the user is authorized to do so.

---

## Variations

### 1. Non-sequential or Obscure IDs (e.g., GUIDs)

Instead of using predictable IDs like `123`, the app may use GUIDs such as: https://insecure-website.com/myaccount?id=a4f9e55a-843d-43c6-8d2e-14be1cbff40a
 

While this makes guessing harder, if these GUIDs are leaked elsewhere (e.g., in comments, messages, or shared links), attackers may still exploit them to access other users' data.

---

### 2. Redirect with Leaked Data

Some applications attempt to block unauthorized access by redirecting users to a login page. However, if the **HTTP response contains sensitive data before the redirect**, an attacker may:

- Intercept the response
- Extract the sensitive data
- Ignore the redirect

Thus, the attack is still successful despite the redirect.

---

## ✅ Mitigation Strategies

- **Enforce access control on the server-side**: Never rely solely on client-side values.
- **Implement strict access validation**: Ensure that users can only access resources they own.
- **Avoid exposing internal IDs**: Use tokens or hashed identifiers instead.
- **Follow proper access control models**: Use RBAC (Role-Based Access Control), ABAC, or similar.
- **Sanitize response content**: Ensure no sensitive data is sent before permission checks or redirects.

---

## Summary

| Component                | Description |
|--------------------------|-------------|
| Vulnerability Type       | Horizontal Privilege Escalation |
| Underlying Issue         | IDOR – Missing proper access control |
| Impact                   | User A can access User B’s data |
| Exploitation             | Modify ID in URL or use exposed GUIDs |
| Mitigation               | Server-side access checks, hidden IDs, proper access control logic |

---

## Further Reading

- [OWASP - Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
- [PortSwigger - Access Control Vulnerabilities](https://portswigger.net/web-security/access-control)




