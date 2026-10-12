# web-api-penetration-test

Web Application & API Penetration Testing - Team Project

## Project Objective

To assess the security of a web application and its APIs in an authorized testing environment, identify vulnerabilities, collect evidence, and recommend security fixes.

## Target Lab

**OWASP Juice Shop** (local Docker instance)

Testing is limited to this authorized lab environment only.

## Team & Vulnerability Assignments

| # | Vulnerability | Owner |
|---|---|---|
| 1 | SQL Injection (SQLi) | **Mina Nabil Shokry Habib** |
| 2 | Cross-Site Scripting (XSS) | **Sayed Ahmed Mohamed Sayed** |
| 3 | Cross-Site Request Forgery (CSRF) | **Rola Refaat Eldaba** |
| 4 | Server-Side Request Forgery (SSRF) | **Samir Samir Eid Rady** |
| 5 | File Upload Vulnerabilities | **Ola Ayman Shoukry Abdelsalam** |
| 6 | Path Traversal & IDOR/BOLA | **Abanoub Ebid Thapt** (Team Leader) |

Each member tests their assigned vulnerability class across the lab, documents findings using the team's Individual Testing Workpaper template, and submits evidence for review before it is added to the final report.

## Testing Schedule

- **Week 1:** Web Fundamentals & Burp Suite Setup
- **Week 2:** Input-Based Vulnerabilities
- **Week 3:** Authentication, Session, Access Control & API Testing
- **Week 4:** Attack Chaining, Reporting & Retesting

## Tools

- Burp Suite
- Web Browser
- GitHub
- OWASP Juice Shop

## Scope

Testing will be limited to the authorized OWASP Juice Shop lab environment. No system outside this lab may be tested.

## Deliverables

- Application Map
- Test Plan
- Vulnerability Findings & Evidence (per owner, per vulnerability class)
- API Testing Results
- Final Penetration Testing Report
- Remediation & Retest Checklist

## Repository Structure

```
web-api-penetration-test/
├── README.md
├── team/              # Members, roles, and workpapers
├── week-1/            # Application map, Burp file, auth flow, test plan
├── week-2/            # Input vulnerability findings and evidence
├── week-3/            # API, auth, access control findings
├── week-4/            # Final report, retest, demo
└── evidence/          # Evidence files, organized by finding ID
```

> ⚠️ Do not commit real passwords, valid cookies/session tokens, or personal data to this repository. Use test accounts and lab-generated data only.

## Team

- **Abanoub Ebid Thapt**
- **Mina Nabil Shokry Habib**
- **Sayed Ahmed Mohamed Sayed**
- **Samir Samir Eid Rady**
- **Rola Refaat Eldaba**
- **Ola Ayman Shoukry Abdelsalam**
