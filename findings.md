# Vulnerability Assessment Findings



**Target:** http://demo.testfire.net (Altoro Mutual demo site, IP 65.61.137.117 at time of scan)
**Date:** 3 November 2026
**Assessor:** Sukh Ram Godara
**Program:** Future Interns, Cyber Security Internship, Task 1
**Approach:** Read-only, passive analysis. No exploitation, no active scanning.

**Tools:** OWASP ZAP 2.17.0 (Safe Mode, passive), Nmap 7.99, Firefox DevTools

## 

## Summary



|Risk|Count|
|-|-|
|High|0|
|Medium|6|
|Low|7|
|Informational|3|



The site has no High-risk findings from passive analysis. The main weaknesses are the lack of encryption by default (HTTP), missing browser security headers, and missing protection on forms (CSRF).



## Medium Risk



|ID|Finding|What it means (plain language)|Source|Recommended fix|
|-|-|-|-|-|
|M-01|Site served over unencrypted HTTP (port 80)|Data sent to the site (such as login or form details) can be read or altered in transit|Nmap, browser|Redirect all HTTP to HTTPS and enable HSTS|
|M-02|Anti-CSRF tokens missing (3 forms, e.g. feedback.jsp)|Another website could make a user submit a form without their knowledge|ZAP|Add a unique anti-CSRF token to every form|
|M-03|Content-Security-Policy header not set|The browser is not told which scripts are trusted, making fake scripts easier to run|ZAP, DevTools|Add a Content-Security-Policy header|
|M-04|Anti-clickjacking header missing|Another site could hide this page inside its own and trick users into clicking|ZAP, DevTools|Set `X-Frame-Options` or CSP `frame-ancestors`|
|M-05|Sub Resource Integrity attribute missing|If an external file is tampered with, this site would load the altered file|ZAP|Add `integrity` attributes to external scripts and styles|
|M-06|HTTPS certificate expired (port 443)|Browsers show a full-page security warning, visitors cannot trust the site, and users get used to clicking through warnings|Browser (Firefox error SEC\_ERROR\_EXPIRED\_CERTIFICATE, expired 6/21/2026)|Renew the certificate and set up automatic renewal|

## 

## Low Risk



|ID|Finding|What it means (plain language)|Source|Recommended fix|
|-|-|-|-|-|
|L-01|Server technology/version exposed (`Apache-Coyote/1.1`)|Tells attackers what software is running, helping them look for known weaknesses|ZAP, DevTools, Nmap|Hide the banner and keep Tomcat updated. Exact version should be verified|
|L-02|`X-Content-Type-Options` header missing|Browser may wrongly guess a file's type|ZAP, DevTools|Add `X-Content-Type-Options: nosniff`|
|L-03|Session cookie without SameSite attribute|Cookie is sent along with requests from other sites|ZAP, DevTools|Set `SameSite=Lax` or `Strict`|
|L-04|Session cookie without Secure flag|Cookie can be sent over unencrypted HTTP|DevTools|Add the `Secure` flag|
|L-05|Debug error messages visible|Errors reveal internal technical detail|ZAP|Show a simple generic error page in production|
|L-06|Cross-domain JavaScript source file inclusion|Site runs JavaScript from outside servers; if one is compromised, so is the site|ZAP|Use trusted sources only, or host files locally|
|L-07|Port 8080 open (alternate web port)|Extra web ports often hold test or admin pages|Nmap|Close it if not needed, or restrict by firewall|

## 

## Informational



|ID|Observation|
|-|-|
|I-01|Suspicious developer comments found in page source|
|I-02|Modern web application detected|
|I-03|Session management response identified|

## 

## Positive Observations



* Session cookie has the `HttpOnly` flag set, so JavaScript cannot read it.
* Nmap showed only 3 open ports (80, 443, 8080); the other 997 scanned ports were filtered.



## Fix Priority



1. Fix HTTPS: renew the expired certificate and redirect HTTP to HTTPS (M-01, M-06, L-04)
2. Add security headers together in server configuration (M-03, M-04, L-02)
3. Add CSRF protection to all forms (M-02)
4. Remaining cleanup: SRI, server banner, debug errors, port 8080, cookie SameSite



## Notes



* Risk ratings are the assessor's classification. M-01, M-06, L-04 and L-07 are manual observations (not raised by ZAP).
* Third-party hosts that appeared in ZAP (github.com, swagger.io hosts) were out of scope and are not assessed.
* Counts match the exported ZAP report for http://demo.testfire.net (12 alerts: 4 Medium, 5 Low, 3 Informational), plus 4 manual observations (M-01, M-06, L-04, L-07), 16 findings in total.
* Evidence (ZAP report HTML, screenshots, Nmap output) is stored in the `evidence/` folder.

