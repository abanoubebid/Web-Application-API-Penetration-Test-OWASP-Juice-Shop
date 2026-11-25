# OWASP Juice Shop – Login Admin (SQL Injection – Authentication Bypass)

> Tested on a local, intentionally vulnerable training app (OWASP Juice Shop) at `http://127.0.0.1:3000`.  
> Date: 2026-11-25 | Tools: Firefox, Burp Suite (Repeater)

## Summary

The login functionality is vulnerable to SQL Injection. An attacker can inject a SQL expression into the email field to bypass authentication and access the administrator account without knowing the administrator's password.

| Field | Value |
|---|---|
| Vulnerability | SQL Injection – Authentication Bypass |
| Category | OWASP Top 10 – A03:2021 Injection |
| Endpoint | `POST /rest/user/login` |
| Authentication | None required before login |
| Impact | Confidentiality / Integrity – unauthorized access to the administrator account |
| Challenge solved | **Login Admin** (Log in with the administrator's user account.) |

## Test Accounts

| Role | Email | User ID | Resource ID | Resource content |
|---|---|---|---|---|
| Attacker | Not required | Not applicable | Not applicable | SQL injection payload |
| Victim | `admin@juice-sh.op` | Not specified | Not applicable | Administrator account |

## Steps to Reproduce

### 1. Open the Login Page

Open the OWASP Juice Shop application and navigate to the login page.

The login form contains an **Email** field and a **Password** field.

![Login Page](images/01-login-page.png)

### 2. Enter the SQL Injection Payload

In the **Email** field, enter the following payload:

```text
' or 1=1--
```

Enter any value in the password field.

![SQL Injection Payload](images/02-sqli-payload.png)

### 3. Submit the Login Request

Click **Log in**.

The application processes the injected SQL expression instead of treating the input only as a normal email address.

![Login Request](images/03-login-request.png)

### 4. Authentication Bypass

The login request is accepted and the application authenticates the attacker as an administrator.

The application displays the successful challenge message:

```text
You successfully solved a challenge: Login Admin
(Log in with the administrator's user account.)
```

![Challenge Solved](images/04-login-admin-solved.png)

### 5. Result

The attacker successfully logged into the administrator account without knowing the administrator's password.

This confirms that the login functionality is vulnerable to SQL Injection authentication bypass.

## Root Cause

The application does not safely handle user-controlled input in the authentication SQL query.

The injected SQL expression:

```text
' or 1=1--
```

changes the logic of the SQL query and allows the authentication condition to evaluate as true.

## Impact

An attacker may be able to bypass authentication and gain unauthorized access to privileged accounts.

If an administrator account is compromised, the attacker may gain access to administrative functionality and sensitive application data.

## Remediation

- Use parameterized SQL queries / prepared statements.
- Never concatenate user input directly into SQL queries.
- Validate and sanitize user input where appropriate.
- Use secure authentication mechanisms.
- Apply least-privilege permissions to the application's database account.

## References

- OWASP Top 10 – A03:2021 Injection
- OWASP SQL Injection
- OWASP Juice Shop

## Disclaimer

For educational purposes only, performed on a local lab environment.
