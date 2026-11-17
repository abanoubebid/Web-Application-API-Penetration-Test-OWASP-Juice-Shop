# OWASP Juice Shop – CSRF on `/profile` (Cross-Site Request Forgery)

> Tested on a local, intentionally vulnerable training app (OWASP Juice Shop) at `http://127.0.0.1:3000`.
>
> Date: 2026-10-27 | Tools: Burp Suite (Repeater), Firefox + PwnFox, Python HTTP Server

## Summary

The `/profile` endpoint allows an authenticated user’s username to be changed without requiring a CSRF token or an additional origin check.

An attacker can craft a request containing a new username and cause it to be submitted while the victim is logged in. The application accepts the request using the victim's existing session and changes the username without the victim interacting with the actual profile form.

The endpoint also accepts the username through a GET parameter, making the issue easier to trigger.

| Field | Value |
|---|---|
| Vulnerability | Cross-Site Request Forgery (CSRF) |
| Category | OWASP Top 10 – A01:2021 Broken Access Control |
| Endpoint | `POST /profile` and `GET /profile?username=...` |
| Authentication | Required – any valid customer account |
| Impact | Integrity – unauthorized modification of the victim's username |
| Challenge solved | **CSRF on /profile** (Change a logged-in user's username without their direct action) |

## Test Accounts

| Role | Email | User ID | Resource | Resource content |
|---|---|---|---|---|
| Attacker | `ddd@gmail.com` | Test account | `/profile` | Username |
| Victim | `egs@gmail.com` | Test account | `/profile` | Username |

> The testing was performed using normal customer accounts. No special privileges were required.

## Steps to Reproduce

### 1. Confirm the authenticated profile

First, log in to OWASP Juice Shop and open the `/profile` page.

The baseline request was captured in Burp Suite and returned `HTTP 200 OK`.

![Profile baseline](images/01-profile-baseline.png)

This confirms that the user is authenticated and that the profile endpoint is accessible.

---

### 2. Change the username using POST

Send a request to `/profile` through Burp Suite Repeater with a username value in the POST body.

Example:

```http
POST /profile HTTP/1.1
Host: 127.0.0.1:3000
Content-Type: application/x-www-form-urlencoded

username=csrf_test_1
```

The server returned:

```http
HTTP/1.1 302 Found
```

and redirected back to `/profile`.

![POST profile request](images/02-post-profile.png)

The username was successfully changed.

---

### 3. Test the endpoint using GET

The same functionality was tested using a GET query parameter:

```http
GET /profile?username=rrrrd HTTP/1.1
Host: 127.0.0.1:3000
```

The application accepted the request and returned `HTTP 200 OK`.

![GET profile request](images/03-get-profile.png)

This shows that the username change was not restricted to a state-changing POST request.

---

### 4. Exploit using a crafted CSRF page

A small HTML proof-of-concept page was created with an auto-submitting form targeting `/profile`.

The PoC was served locally using:

```bash
python3 -m http.server 8000
```

The page automatically submitted the request without requiring the victim to manually interact with the profile form.

![CSRF PoC](images/04-csrf-poc.png)

While the victim was already logged in to Juice Shop, opening the PoC caused the username-change request to be sent using the existing authenticated session.

The request resulted in:

```http
POST /profile HTTP/1.1
Host: 127.0.0.1:3000
```

The important point is that the request was accepted without a CSRF token.

---

### 5. Verify the result

After executing the PoC, the victim's `/profile` page was opened again.

The username had been changed to:

```text
csrf_test_2
```

without manually submitting the original profile form.

![Username changed](images/05-profile-changed.png)

This confirms that the crafted request was able to perform the state-changing action using the authenticated session.

---

### 6. Repeat the test using a local HTML file

The same PoC was also tested directly as a local file:

```text
file:///home/rola/csrf-username.html
```

The username was changed to:

```text
csrf-lab-test
```

after opening the local HTML file.

![Local file PoC](images/06-local-file-poc.png)

---

### 7. Confirm the request chain

Browser DevTools was used to confirm the request flow.

The observed sequence was:

```text
POST /profile
      ↓
HTTP 302 Found
      ↓
GET /profile
      ↓
HTTP 200 OK
```

![DevTools Network](images/07-devtools-network.png)

This confirms the request chain responsible for the username modification.

## Relevant Request

The vulnerable functionality can be triggered through an authenticated request to `/profile`.

Example:

```http
POST /profile HTTP/1.1
Host: 127.0.0.1:3000
Content-Type: application/x-www-form-urlencoded
Authorization: Bearer <authenticated-token>

username=csrf_test_2
```

The important security issue is that the server does not require a CSRF token tied to the user's session before processing the state-changing request.

## Expected vs. Actual Result

### Expected

A state-changing request should require CSRF protection, such as:

- A CSRF token associated with the user's session.
- Proper Origin/Referer validation.
- Appropriate SameSite cookie configuration.
- State-changing operations should not be performed through GET requests.

### Actual

The application accepted the username-change request without a CSRF token.

The GET variant was also accepted:

```http
GET /profile?username=rrrrd
```

and the username was changed using the existing authenticated session.

## Root Cause

The root cause is the lack of proper CSRF protection on the `/profile` state-changing functionality.

The server accepts the username-change request without verifying that the request originated from the legitimate application.

Additionally, accepting username changes through a GET request makes the functionality unnecessarily exposed to unintended requests.

## Impact

An attacker could potentially cause a logged-in victim to change their username without the victim intentionally submitting the profile form.

This affects the **integrity** of the user's profile data.

The issue could also become more serious if similar CSRF weaknesses existed on other sensitive endpoints, such as account settings or security-related operations.

During this test, only the username-change functionality was demonstrated. No password change or full account takeover was demonstrated.

## Remediation

- Implement a server-side CSRF token for state-changing requests.
- Validate the CSRF token against the user's authenticated session.
- Validate the `Origin` and/or `Referer` headers where appropriate.
- Configure authentication cookies with an appropriate `SameSite` attribute.
- Do not allow state-changing operations through GET requests.
- Restrict username changes to POST/PUT/PATCH requests.
- Re-test the endpoint from a genuinely separate origin after remediation.

## References

- OWASP Top 10 – A01:2021 Broken Access Control
- OWASP Cross-Site Request Forgery Prevention Cheat Sheet
- OWASP Juice Shop

## Disclaimer

For educational purposes only.

This vulnerability was tested against a locally hosted, intentionally vulnerable OWASP Juice Shop training environment at `http://127.0.0.1:3000`.

No real users, production systems, or third-party websites were targeted.
