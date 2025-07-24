
# Broken Access Control (BAC) Scenarios

## 1. Referer-based Access Control

### Explanation:
Some websites implement access control based on the `Referer` header included in HTTP requests.

- The `Referer` header is added by browsers to indicate the page from which a request originated.
- For example, a request from `/admin` to `/admin/deleteUser` may include `Referer: https://example.com/admin`.

### Vulnerability:
If a system only checks the `Referer` header to validate access, it becomes vulnerable:

- An attacker can forge a direct request to a sensitive URL like `/admin/deleteUser`.
- By manually setting the `Referer` header to `/admin`, they can bypass access controls.
- Since the `Referer` header is under full client control, this type of access control is insecure.

### Example of a Forged Request:

```http
GET /admin/deleteUser?id=123 HTTP/1.1
Host: vulnerable.com
Referer: https://vulnerable.com/admin
Cookie: session=attackersession
```

> The system assumes the request originated from the admin panel and allows unauthorized actions.

### Summary:
Relying on the `Referer` header for access control is unsafe, as it can be easily manipulated by attackers.

---

## 2. Location-based Access Control

### Explanation:
Some websites (e.g., banking or media platforms) restrict access based on the user's geographic location.

- Access may be limited to users from certain countries or regions due to legal or business constraints.

### Vulnerability:
This control mechanism can often be bypassed through:

- **VPNs or Proxies**: Change the user's IP address to a permitted region.
- **Client-side Geolocation Manipulation**: Modify location data via browser scripts.

### Example of a Location Spoofing Attack:

```javascript
navigator.geolocation.getCurrentPosition = function(success) {
  success({ coords: { latitude: 10.762622, longitude: 106.660172 } }); // Ho Chi Minh City coordinates
};
```

> The application thinks the user is in Vietnam and grants access.

### Summary:
Geolocation-based access control is easily bypassed if it relies solely on client-side data or IP-based location.

---

## Conclusion

| BAC Type                      | Core Issue                                             | Attack Method                            |
|------------------------------|---------------------------------------------------------|-------------------------------------------|
| Referer-based Access Control | Trusting `Referer` header for access control            | Forging the `Referer` header              |
| Location-based Access Control| Using unreliable client-side data or IP for geolocation | VPN, proxy, or location spoofing via JS   |

**Security Recommendation:** Always enforce access control on the server-side using authenticated sessions and user roles. Never rely on headers or client-side data for security decisions.

## Further Reading

- [OWASP - Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
- [PortSwigger - Access Control Vulnerabilities](https://portswigger.net/web-security/access-control)