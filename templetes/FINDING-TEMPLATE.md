# <App name> – <Vulnerability title> (<Vulnerability type / Category>)

> Tested on a local, intentionally vulnerable training app (<App name>) at `<base URL>`.
> Date: <YYYY-MM-DD> | Tools: <Burp Suite (Repeater), Firefox + PwnFox>

## Summary

<The endpoint `<METHOD /path/{param}>` ... without checking ... An authenticated user can ... just by changing ...>

| Field | Value |
|---|---|
| Vulnerability | <Vulnerability name (ABBR)> |
| Category | <OWASP Top 10 – A0X:2021 Category name> |
| Endpoint | `<METHOD /path/{param}>` |
| Authentication | <Required (any valid user) / None / ...> |
| Impact | <Short impact (confidentiality / integrity / availability)> |
| Challenge solved | **<Challenge name>** (<Challenge description>) |

## Test Accounts

| Role | Email | User ID | <Resource> ID | <Resource> content |
|---|---|---|---|---|
| Attacker | <email> | <id> | <id> | <content> |
| Victim | <email> | <id> | <id> | <content> |

## Steps to Reproduce

### 1. <Step title>

<What this step shows.>

![<Caption>](images/01-<caption>.png)
![<Caption>](images/02-<caption>.png)

### 2. <Step title>

<What this step shows.>

![<Caption>](images/03-<caption>.png)

### 3. <Step title>

<What this step shows.>

![<Caption>](images/04-<caption>.png)

### 4. <Exploit step title>

<What was changed, using which token/header>:

```http
<METHOD /path HTTP/1.1>
Host: <host>
Authorization: Bearer <victim_token>
X-User-Email: <email>
Cookie: token=<victim_token>
```

<What the server responds with and why it proves the issue>:

![<Caption>](images/05-<caption>.png)

Relevant part of the response:

```json
{
  <trimmed response>
}
```

### 5. Result

<Final confirmation of the issue / challenge message>

## Root Cause

<Why it happens.>

## Impact

<Who can do what, at what scale.>

## Remediation

- <Fix 1>
- <Fix 2>
- <Fix 3>
- <Fix 4>

## References

- OWASP Top 10 – <A0X:2021 Category>
- OWASP API Security Top 10 – <APIX:2023 Category>
- <App link>

## Disclaimer

For educational purposes only, performed on a local lab environment.
