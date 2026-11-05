# OWASP Juice Shop – File Upload Behavior (Unconfirmed)

> Tested on a local, intentionally vulnerable training app (OWASP Juice Shop) at `http://127.0.0.1:3000`.
> Date: 2026-10-28 to 2026-11-04 | Tools: Burp Suite Community Edition (Repeater / Proxy), Firefox

## Summary

The `POST /file-upload` endpoint accepted multipart upload requests containing different file content types and returned `HTTP/1.1 204 No Content` in the captured tests. The supplied screenshots show a plain-text upload (`test.txt`, `Content-Type: text/plain`) and a request whose uploaded content is ZIP-formatted (`PK` signature, `Content-Type: application/zip`) while the multipart filename is `test.txt`. The evidence does not show whether the uploaded files were stored, publicly retrievable, served with an unsafe content type, or executed. Therefore, an unrestricted or dangerous file upload vulnerability is **not confirmed** by the supplied evidence.

| Field | Value |
|---|---|
| Vulnerability | File Upload Behavior (unconfirmed) |
| Category | Not specified — a vulnerability category cannot be assigned from the current evidence |
| Endpoint | `POST /file-upload` |
| Authentication | Not specified (a session cookie was present in the captured requests; its validity and authorization requirements are not established) |
| Impact | Not established; the screenshots show successful request handling only |
| Challenge solved | Not specified (no challenge completion message is visible) |

## Test Accounts

| Role | Email | User ID | Uploaded file ID | Uploaded file content |
|---|---|---|---|---|
| Attacker | Not specified | Not specified | Not specified | Not specified |
| Victim | Not specified | Not specified | Not specified | Not specified |

## Steps to Reproduce

### 1. Open the Complaint page

The browser is open to the local OWASP Juice Shop instance at `127.0.0.1:3000/#/complain`. The form contains a customer field, message field, and invoice file selector. This establishes the visible upload-related UI, but does not by itself establish a vulnerability.

![Complaint form](images/01-complaint-form.png)

### 2. Capture the initial upload request

Burp Suite captures a `POST` request to `/file-upload` on `127.0.0.1:3000`. The request uses `multipart/form-data` and the captured response is `204 No Content`. The screenshot shows an upload named `test.zip` with ZIP content.

![Initial upload request](images/02-initial-upload-request.png)

### 3. Submit a plain-text file

A Repeater request shows a multipart part with `filename="test.txt"`, `Content-Type: text/plain`, and the body `this is a plain text upload test`. The server responds with `HTTP/1.1 204 No Content`.

![Plain-text upload request and response](images/03-plain-text-upload.png)

### 4. Submit ZIP-formatted content

A second Repeater request uses the multipart filename `test.txt` but declares `Content-Type: application/zip`; the body begins with the ZIP signature `PK`. The server again responds with `HTTP/1.1 204 No Content`. This demonstrates acceptance/handling of the submitted request, not unsafe storage, retrieval, or execution.

![ZIP-formatted upload request and response](images/04-zip-content-upload.png)

### 5. Result

The supplied captures show `204 No Content` responses for the upload requests. No response body, file URL, stored-file listing, retrieval test, execution result, or Juice Shop challenge-success message is shown. The current evidence is therefore insufficient to confirm `Unrestricted File Upload` or assign a CWE, OWASP category, severity, or CVSS score.

## Root Cause

Not determined. The screenshots do not include server-side implementation details or evidence of how uploaded files are validated, stored, named, or served.

## Impact

Not established from the supplied evidence. A `204 No Content` response only indicates that the server returned that status for the request; it does not prove that a file was stored or that an attacker can access or execute it.

## Remediation

The following are general defensive recommendations, not a confirmed root-cause fix for this instance:

- Enforce an allowlist of permitted file extensions and verify file signatures/content rather than trusting the client-provided `Content-Type`.
- Generate server-side filenames and store uploads outside the web root or in non-executable object storage.
- Apply strict size limits and safely handle archive extraction, including path traversal and decompression-bomb protections.
- Require authorization for upload and download operations, and serve uploaded content with safe content-disposition and content-type headers.

## References

- OWASP File Upload Cheat Sheet
- CWE-434 – Unrestricted Upload of File with Dangerous Type (potential mapping only; not confirmed by this evidence)
- OWASP Juice Shop – `http://127.0.0.1:3000` (local lab instance)

## Disclaimer

For educational purposes only, performed on a local lab environment.
