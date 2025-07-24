# Vertical Privilege Escalation

Vertical privilege escalation occurs when a low-privileged user (e.g., regular user) can access or perform actions reserved for higher-privileged users (e.g., admin). This is a critical form of Broken Access Control (BAC).

---

## 1. Unprotected Functionality

### Description

Admin features are accessible directly via URL, even if the interface hides the link.

### Example

```
https://insecure-website.com/admin
```

A regular user can access it because there is no access control on the backend.

### Protection

* Always verify access rights on the server side.
* Do not rely solely on hiding UI elements.

---

## 2. Security by Obscurity

### Description

Admin URLs are made hard to guess but lack proper access control.

### Example

```
https://insecure-website.com/administrator-panel-yb556
```

This URL might be leaked via JavaScript:

```html
<script>
  var isAdmin = false;
  if (isAdmin) {
    var adminPanelTag = document.createElement('a');
    adminPanelTag.setAttribute('href', 'https://insecure-website.com/administrator-panel-yb556');
  }
</script>
```

### Protection

* Never rely on obscurity for security.
* Enforce access control on the backend.

---

## 3. Parameter-based Access Control

### Description

The system stores access rights in user-controllable values like query strings, cookies, or hidden fields.

### Example

```
https://insecure-website.com/login/home.jsp?admin=true
```

Or in cookies:

```
role=admin
```

### Protection

* Never trust client-side data.
* Store and verify roles on the server side (session/token).

---

## 4. Platform Misconfiguration

### Description

Access control is implemented via frontend routing rules but can be bypassed using headers or alternate methods.

### Example

Request:

```http
POST / HTTP/1.1
X-Original-URL: /admin/deleteUser
```

Backend trusts `X-Original-URL`, allowing a bypass.

### Protection

* Avoid trusting headers like `X-Original-URL` or `X-Rewrite-URL`.
* Enforce access control on the backend consistently.

---

## 5. HTTP Method Bypass

### Description

The system blocks certain methods (e.g., POST) but the backend accepts others.

### Example

Blocked:

```
POST /admin/deleteUser
```

But attacker sends:

```
GET /admin/deleteUser
```

### Protection

* Strictly define and validate allowed HTTP methods.
* Backend must validate both method and path.

---

## 6. URL Matching Discrepancies

### Description

Backends may accept variants of the same URL, while access control fails to cover them all.

### Example

* `/ADMIN/deleteUser` vs `/admin/deleteUser`
* `/admin/deleteUser.anything`
* `/admin/deleteUser/`

### Protection

* Normalize paths before access checks.
* Ensure consistent routing and access control.
* In Spring Framework: disable `useSuffixPatternMatch` (enabled by default before Spring 5.3).

---

## Summary: General Protection Guidelines

* ✅ **Always enforce access control on the server side.**
* ❌ **Do not rely on UI or obscurity for protection.**
* ✅ **Never trust data from the client side.**
* ✅ **Log and alert on unauthorized access attempts.**
* ✅ **Cover all HTTP methods and URL path variants.**

## Further Reading

- [OWASP - Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
- [PortSwigger - Access Control Vulnerabilities](https://portswigger.net/web-security/access-control)
---


