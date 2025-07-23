# Broken Access Control (BAC)

## Understanding Access Control

Access control defines who or what can perform specific actions or access particular resources.  
Within web applications, access control is built upon three key layers:

- Authentication: Verifies a user's identity.
- Session Management: Associates incoming HTTP requests with a particular authenticated user.
- Authorization (Access Control): Determines whether the user has permission to execute the requested action.

When access control mechanisms fail or are misconfigured, attackers can bypass restrictions, leading to Broken Access Control, which is one of the most serious and widespread vulnerabilities on modern websites.

Designing effective access controls is challenging because it requires balancing technical enforcement with business logic, organizational rules, and legal requirements.  
Human error in designing these rules often results in security flaws.

---

## Best Practices to Prevent Access Control Issues

To reduce the risk of Broken Access Control, implement a layered defense strategy with the following principles:

1. Do not depend on obscurity alone. Security through obscurity is insufficient for access control.  
2. Default to deny. Ensure that all resources are restricted unless explicitly made public.  
3. Centralize authorization checks. Use a unified, application-wide mechanism for enforcing permissions.  
4. Enforce explicit permission declarations in code. Developers should specify allowed actions, with all else being denied automatically.  
5. Perform continuous testing and audits. Regular reviews help verify that access rules are both effective and up-to-date.

---

## Why BAC Is Critical

- Allows attackers to gain unauthorized access to sensitive data or administrative functionality.  
- Can lead to horizontal escalation (accessing other users' data) or vertical escalation (gaining higher privileges).  
- Frequently exploited in real-world attacks due to its complexity and prevalence.

---

## Useful Resources

- [OWASP Top 10: Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
- [PortSwigger Access Control Learning Labs](https://portswigger.net/web-security/access-control)
