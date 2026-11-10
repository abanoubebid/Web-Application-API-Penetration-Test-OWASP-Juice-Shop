\*\*DOM-Based XSS — OWASP Juice Shop\*\*



\## 1. Overview



\- \*\*Target:\*\* OWASP Juice Shop

\- \*\*Vulnerability:\*\* DOM-Based Cross-Site Scripting (XSS)

\- \*\*Environment:\*\* Kali Linux

\- \*\*Tools:\*\* Firefox, Burp Suite (optional)



!\[OWASP Juice Shop](screenshots/01-juice-shop.png)



\## 3. Exploitation



I entered the following payload into the search field:



```

<iframe src="javascript:alert(`xss`)">

```



After submitting the input, a JavaScript alert appeared in the browser, confirming that the payload was executed.



!\[XSS Payload Execution](screenshots/02-xss-payload.png)



\## 4. Verification



\- Opened the Score Board.

\- Confirmed that the DOM XSS challenge was marked as solved.

\- The challenge appeared under the XSS category.



!\[Score Board Verification](screenshots/03-scoreboard.png)









