# Security Scan Analysis - [7 Dec 2025]
# Exercise 2,3,4

## Exercise 2

## DAST Results (ZAP Baseline)
- Total URLs scanned: 3
- PASS: 61 checks
- WARN-NEW: 9 warnings
- FAIL-NEW: 0 failures

### Warning Summary
Missing Anti-clickjacking Header [10020] x 1
X-Content-Type-Options Header Missing [10021] x 1 
Server Leaks Version Information via "Server" HTTP Response Header Field [10036] x 3 
Content Security Policy (CSP) Header Not Set [10038] x 3 
Storable and Cacheable Content [10049] x 3 
Permissions Policy Header Not Set [10063] x 3
Modern Web Application [10109] x 1 
Insufficient Site Isolation Against Spectre Vulnerability [90004] x 3 
Sec-Fetch-Dest Header is Missing [90005] x 12 

## SCA Results (Dependency Check)
- High severity: 0
- Medium severity: 0
- Low severity: 0

### Vulnerable Packages
None

## SAST Results (CodeQL)
- Total issues: 1
By type: High: 1
Medium: 0
Low: 0
Critical: 0

## Recommendations
Based on my findings, the SAST finding should be of immediate priority as the risk level is high. This  application is running on debug = true. In a production environment, the Flask debugger allows an attacker to execute arbitrary Python code directly in the browser (Remote Code Execution). This grants them full control over the server. 

## Exercise 3
Code	Issue	Risk	Affected	Why It Matters
10020	Missing X-Frame-Options	Medium	http://localhost:5000	Prevents clickjacking attacks

10021	X-Content-Type-Options missing	Low	http://localhost:5000	Prevents MIME-sniffing attacks

10036   Server Leaks Version Information via "Server" HTTP Response Header Field    Low      http://localhost:5000,http://localhost:5000/robots.txt, http://localhost:5000/sitemap.xml   Prevents attackers from identifying other vulnerabilities your web/application server is subject to.

10038   Content Security Policy (CSP) Header Not Set    Medium  http://localhost:5000    helps to detect and mitigate certain types of attacks, including Cross Site Scripting (XSS) and data injection attacks

10049   Storable and Cacheable Content  Informational   http://localhost:5000, http://localhost:5000/robots.txt, http://localhost:5000/sitemap.xml  prevents attacker on the same network to view another user's private data or potentially hijack their session."

10063   Permissions Policy Header Not Set   Low     http://localhost:5000, http://localhost:5000/robots.txt, http://localhost:5000/sitemap.xml  ensures the user privacy by limiting or specifying the features of the browsers can be used by the web resources

10109    Modern Web Application     Low     http://localhost:5000   Test Coverage and False Negatives.

90004   Insufficient Site Isolation Against Spectre Vulnerability   Low     http://localhost:5000    prevents malicious websites open in the user's browser from exploiting CPU vulnerabilities (like Spectre) to read sensitive data from your application's memory.

90005   Sec-Fetch-Dest Header is Missing    Informational    http://localhost:5000,http://localhost:5000/robots.txt, http://localhost:5000/sitemap.xml      Specifies how and where the data would be used

## Exercise 4
# Scanner Comparison

## Only CodeQL (SAST) Found
Focus: Source code, logic errors, and insecure configurations

Identified finding: Flask Debug Mode enabled (debug=True)

Why only SAST detected it:

DAST (ZAP) interacts with the running app but the Baseline Scan did not trigger errors that reveal the debug PIN or stack traces.

SCA ignores this because it is not a third-party package vulnerability—it is a developer-written configuration in app.py.

## Only ZAP (DAST) Found
Focus: Runtime behavior, HTTP responses, and server configuration

Identified findings:

Missing security headers: CSP, X-Frame-Options, X-Content-Type-Options

Information leakage: Server revealing “Werkzeug/2.0.1 Python/3.9”

Cache misconfiguration: Missing Cache-Control headers

Why only DAST detected it:

SAST analyzes code but cannot confirm the final HTTP response, since headers may be inserted by frameworks or middleware at runtime.

SCA does not inspect HTTP traffic or runtime behavior.

## Only Dependency-Check (SCA) Found
Focus: Third-party libraries and supply chain risks

Identified findings:

Requirements scanned and confirmed to have 0 vulnerable packages

Why only SCA detected it:

SAST reviews developer-written code but ignores internal library code.

DAST tests behavior and cannot distinguish safe/vulnerable library versions unless behavior differs.

## All Three Tools
they focus on different layers so there isnt full overlap
However, both DAST and SCA detected the Werkzeug version (one via headers, one via requirements). Although this is not true overlap, +they detected the same information for different reasons, not the same vulnerability.