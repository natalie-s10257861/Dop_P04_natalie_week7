# Security Scan Analysis - [Date]

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
- High severity: 1
- Medium severity: 0
- Low severity: 0

### Vulnerable Packages
Flask app is run in debug mode

## SAST Results (CodeQL)
- Total issues: X
- By type: [breakdown]

## Recommendations
[Based on findings, what should be prioritized?]