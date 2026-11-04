# HTTP Response Header Check



**Target:** http://demo.testfire.net/
**Method:** Firefox Developer Tools, Network tab, Response Headers (Raw view)
**Date:** 3 November 2026



|Header|Present?|Value / Note|
|-|-|-|
|Content-Security-Policy|No|Not set (finding M-03)|
|X-Frame-Options|No|Not set (finding M-04)|
|X-Content-Type-Options|No|Not set (finding L-02)|
|Strict-Transport-Security|No|Site is served over HTTP, so HSTS is not in use (finding M-01)|
|Referrer-Policy|No|Not set|
|Server|Yes|`Apache-Coyote/1.1` (version exposed, finding L-01)|

## 

## Other headers observed



* `Set-Cookie: JSESSIONID=...; Path=/; HttpOnly` (no `Secure` flag, no `SameSite` attribute: findings L-04, L-03)
* `Content-Type: text/html;charset=ISO-8859-1`

## 

## Screenshots



* `03-devtools-response-headers.png`
* `04-devtools-cookies.png`

