# FUTURE\_CS\_01: Vulnerability Assessment Report for a Live Website



**Future Interns | Cyber Security Internship | Task 1
Author:** Sukh Ram Godara
**Date of assessment:** 3 November 2026



## Overview



This repository contains a read-only, passive vulnerability assessment of a public demo website. The goal was to identify common web security weaknesses, classify them as Low / Medium / High, explain them in simple business language, and suggest practical fixes.

**Final report:** [`report/Vulnerability\_Assessment\_Report.pdf`](report/Vulnerability_Assessment_Report.pdf)

## 

## Website Tested



|Item|Detail|
|-|-|
|Target|http://demo.testfire.net (Altoro Mutual demo banking site)|
|IP at time of scan|65.61.137.117|
|Why this target|A public demo site published for security testing, so assessment is permitted|

## 

## Scope



**In scope**

* Public-facing pages of demo.testfire.net only
* Passive analysis of pages browsed manually
* HTTP response header and cookie review
* Basic port and service detection



**Not performed**

* Login bypass, exploitation, brute force, or denial of service
* ZAP active scan or spider, form payload injection (SQLi, XSS)
* Testing of any other host or domain



Third-party hosts that appeared during browsing (github.com, swagger.io) were outside scope and were not assessed. See [`scope.txt`](scope.txt).

## 

## Tools Used



* **OWASP ZAP 2.17.0**: Safe Mode, passive scan through manual browsing
* **Nmap 7.99**: `nmap -sV -T3 demo.testfire.net`
* **Firefox Developer Tools**: response headers and cookies
* **Kali Linux** (virtual machine)
* **Canva**: report design



## Summary of Findings



|Risk|Count|
|-|-|
|High|0|
|Medium|6|
|Low|7|
|Informational|3|



16 findings in total: 12 raised by OWASP ZAP and 4 manual observations (M-01, M-06, L-04, L-07).



**Medium**

* M-01 Site served over unencrypted HTTP (port 80)
* M-02 Anti-CSRF tokens missing on forms
* M-03 Content-Security-Policy header not set
* M-04 Anti-clickjacking header missing
* M-05 Sub Resource Integrity attribute missing
* M-06 HTTPS certificate expired (port 443)



**Low**

* L-01 Server technology and version exposed
* L-02 X-Content-Type-Options header missing
* L-03 Session cookie without SameSite attribute
* L-04 Session cookie without Secure flag
* L-05 Debug error messages visible
* L-06 Cross-domain JavaScript source file inclusion
* L-07 Port 8080 open (alternate web port)



**Positive observations:** the session cookie has the HttpOnly flag, and only 3 of the scanned ports were open (80, 443, 8080).

Full details, plain-language explanations and fixes are in [`findings.md`](findings.md) and the PDF report.

## 

## Repository Structure

```
FUTURE\_CS\_01/
├── README.md
├── scope.txt
├── findings.md
├── report/
│   └── Vulnerability\_Assessment\_Report.pdf
└── evidence/
    ├── nmap-scan.txt
    ├── 2026-11-03-ZAP-Report-demo\_testfire\_net.html
    └── (screenshots)
```



## Ethics Statement



All testing was read-only and passive on a site built for security testing. No credentials were used, no data was modified, and no exploitation was attempted.

## 

## Disclaimer



This work was done for learning purposes as part of the Future Interns Cyber Security internship. Findings describe a deliberately vulnerable demo site and do not represent any real organization.

