# OWASP Juice Shop – CSRF-01: Change Password Without Current Password (CSRF)

> Tested on a local, intentionally vulnerable training app (OWASP Juice Shop) at `http://localhost:3000`.
>
> Date: 2026-11-16 | Tools: Burp Suite Repeater, Firefox

## Summary

The `/rest/user/change-password` endpoint allows an authenticated user to change their password through a `GET` request without requiring a valid current password and without any CSRF token.

The endpoint accepts the `current`, `new`, and `repeat` password values as query-string parameters.

During testing, the request succeeded both when the `current` parameter was completely missing and when it contained an incorrect value.

Because the endpoint uses `GET` and does not implement CSRF protection, the same authenticated request could potentially be triggered from another site using the victim's existing session.

| Field | Value |
|---|---|
| Vulnerability | Cross-Site Request Forgery (CSRF) |
| Finding ID | **CSRF-01** |
| Category | OWASP Top 10 – A01:2021 Broken Access Control |
| Endpoint | `GET /rest/user/change-password` |
| Authentication | Required – authenticated customer account |
| Impact | High – unauthorized password change / potential account takeover |
| Challenge solved | **Change Password CSRF** (Password change can be triggered without CSRF protection) |

## Test Accounts

| Role | Email | User ID | Resource | Resource content |
|---|---|---|---|---|
| Test Account 1 | `ddfd@gmail.com` | `27` | `/rest/user/change-password` | Password |
| Test Account 2 | `egrfs@gmail.com` | Test account | `/rest/user/change-password` | Password |

> Testing was performed only against locally registered test accounts in the OWASP Juice Shop training environment.

## Steps to Reproduce

### 1. Send the request without the current password

First, capture the legitimate password-change request using Burp Suite Repeater.

The `current` parameter was removed completely while keeping the authenticated Bearer token.

Example:

```http
GET /rest/user/change-password?new=fgzH258+&repeat=fgzH258+ HTTP/1.1
Host: localhost:3000
Authorization: Bearer <authenticated-token>
```

The request was accepted even though no current password was supplied.

![Request without current password](images/01-no-current-password.jpg)

The server returned `HTTP 200 OK`.

This demonstrates that the endpoint does not require the `current` password parameter to be present.

---

### 2. Send an incorrect current password

The test was repeated using a deliberately incorrect current password.

Example:

```http
GET /rest/user/change-password?current=Rasd2&new=asdf258+&repeat=asdf258+ HTTP/1.1
Host: localhost:3000
Authorization: Bearer <authenticated-token>
```

`Rasd2` was not the account's real current password.

![Wrong current password - Rasd2](images/02-wrong-current-Rasd2.jpg)

The server still accepted the request with `HTTP 200 OK`.

This demonstrates that supplying an incorrect current password does not prevent the password change.

---

### 3. Repeat with another incorrect password

The same test was repeated using another made-up current password:

```http
GET /rest/user/change-password?current=R222r+&new=fgzH258+&repeat=fgzH258+ HTTP/1.1
Host: localhost:3000
Authorization: Bearer <authenticated-token>
```

![Wrong current password - R222r](images/03-wrong-current-R222r.jpg)

Again, the request was accepted.

This confirms that the behavior is reproducible and is not limited to one specific incorrect value.

---

### 4. Verify that the password was actually changed

The response returned the user's object and showed that the `updatedAt` timestamp had changed.

![Password updated response](images/04-password-updated-response.jpg)

Relevant response data:

```json
{
  "user": {
    "id": 27,
    "username": "ddfd@gmail.com",
    "role": "customer",
    "createdAt": "2026-11-16T12:04:25.867Z",
    "updatedAt": "2026-11-16T12:22:03.283Z"
  }
}
```

The `updatedAt` value moved from the original
