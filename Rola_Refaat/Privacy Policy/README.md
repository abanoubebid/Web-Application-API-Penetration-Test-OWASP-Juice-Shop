# OWASP Juice Shop – Privacy Policy (Information Disclosure / Challenge)

> Tested on a local, intentionally vulnerable training app (OWASP Juice Shop) at `http://127.0.0.1:3000`.  
> Date: 2026-11-25 | Tools: Firefox

## Summary

The **Privacy Policy** challenge is completed by accessing and reading the application's Privacy Policy page.

Unlike a traditional security vulnerability, this challenge does not require exploiting an endpoint or bypassing an authentication mechanism. The challenge is triggered when the user visits the Privacy Policy page.

| Field | Value |
|---|---|
| Vulnerability | Privacy Policy Challenge |
| Category | Not applicable – Juice Shop Awareness / Tutorial Challenge |
| Endpoint | `GET /#/privacy-security/privacy-policy` |
| Authentication | Required in the tested session |
| Impact | Not applicable |
| Challenge solved | **Privacy Policy** (Read our privacy policy.) |

## Test Accounts

| Role | Email | User ID | Resource ID | Resource content |
|---|---|---|---|---|
| Tester | `admin@juice-sh.op` | Not specified | Not applicable | Privacy Policy |
| Victim | Not applicable | Not applicable | Not applicable | Not applicable |

## Steps to Reproduce

### 1. Open OWASP Juice Shop

Open the local OWASP Juice Shop instance:

```text
http://127.0.0.1:3000
```

The application is running locally as an intentionally vulnerable training environment.

![OWASP Juice Shop](images/01-juice-shop.png)

### 2. Open Privacy & Security

Open the application menu and navigate to:

```text
Privacy & Security → Privacy Policy
```

![Privacy Policy Menu](images/02-privacy-policy-menu.png)

### 3. Read the Privacy Policy

The Privacy Policy page is displayed.

The page contains information about the collection, use, and disclosure of personal data.

![Privacy Policy Page](images/03-privacy-policy-page.png)

### 4. Challenge Completion

After opening the Privacy Policy, the application displays the following confirmation:

```text
You successfully solved a challenge: Privacy Policy
(Read our privacy policy.)
```

![Privacy Policy Challenge Solved](images/04-privacy-policy-solved.png)

### 5. Result

The **Privacy Policy** challenge was successfully completed.

No exploitation payload, modified request, or authentication bypass was required.

## Root Cause

Not applicable.

This is a Juice Shop challenge designed to verify that the user accesses and reads the application's Privacy Policy.

## Impact

No direct security impact was demonstrated by this challenge.

The challenge is primarily intended for security-awareness and application-navigation purposes.

## Remediation

For a real application, the Privacy Policy should:

- Clearly explain how user data is collected and processed.
- Explain what personal information is stored.
- Explain how users can request data access or deletion.
- Keep the policy accurate and up to date.
- Make the Privacy Policy easily accessible to users.

## References

- OWASP Juice Shop
- Privacy Policy Challenge

## Disclaimer

For educational purposes only, performed on a local lab environment.
