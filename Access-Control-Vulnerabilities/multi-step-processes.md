# Broken Access Control: Multi-Step Process Vulnerability

## Overview

Web applications often implement critical functions through a **multi-step process**. These steps are used to:

* Collect various inputs or user decisions.
* Allow users to review and confirm before executing an action.

However, vulnerabilities may arise when **access control checks are only enforced on some of the steps**, while others are left unprotected.

---

## Vulnerability Scenario

Consider an administrative function for updating user details. The process consists of three steps:

1. **Load the form:**

   * `GET /admin/user/edit?id=123`
   * Retrieves and displays user details.
   * Access control is enforced ✅

2. **Submit the changes:**

   * `POST /admin/user/submit-edit`
   * Receives edited information.
   * Access control is enforced ✅

3. **Confirm and apply changes:**

   * `POST /admin/user/confirm-edit`
   * Final confirmation and data persistence.
   * **Access control is NOT enforced** ❌

### What goes wrong?

The application assumes that only legitimate users who completed steps 1 and 2 can reach step 3. However, if an attacker discovers the endpoint for step 3, they can **bypass prior steps** and directly trigger the final action.

#### Attack Example:

```http
POST /admin/user/confirm-edit
Content-Type: application/json

{
  "userId": "123",
  "email": "attacker@example.com",
  "role": "admin"
}
```

Even if the attacker is not an admin, the lack of access control on the confirmation step allows them to execute unauthorized changes.

---

## Why This Is Dangerous

* Assumes that all users follow the intended workflow.
* Allows attackers to reconstruct requests and skip access-protected steps.
* Common mistake when relying only on front-end enforcement.

---

## Mitigation Strategies

* **Always enforce access control** at **every step**, especially those that perform the final action.
* Never assume the user followed the legitimate process.
* Use **session state or temporary tokens** to validate multi-step workflows.
* Log and audit all sensitive endpoints, regardless of their position in the flow.

---

## Conclusion

Access control must be consistently applied across all endpoints, especially in multi-step processes. Skipping access checks at any step opens the door for attackers to perform unauthorized actions by sending crafted requests directly to the unprotected endpoints.

**Tip for pentesters:** Focus on endpoints performing critical actions and check if they enforce access control individually, regardless of the steps before them.

## Further Reading

- [OWASP - Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
- [PortSwigger - Access Control Vulnerabilities](https://portswigger.net/web-security/access-control)
