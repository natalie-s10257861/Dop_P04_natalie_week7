# Security Scanning

## Overview
This repository uses three types of security scanning, SAST, SCA, and DAST to ensure the security of the application code, dependencies, and runtime environment. We use three distinct types of security scanning automation.

### SAST (Static Analysis with CodeQL)
- Scans source code for vulnerabilities
- Runs on push to main branch
- Checks for:
  - Security misconfigurations (e.g., Flask app run in debug mode)
  - Code injection vulnerabilities (SQLi, XSS)
  - Hardcoded credentials and secrets
  - Unsafe data flow and handling
- Artifacts: See GitHub Security tab

### SCA (Dependency Check)
- Scans Python packages for known CVEs
- Runs on push to main branch
- Checks for:
  - Known vulnerabilities (CVEs) in third-party libraries listed in requirements.txt
  - Outdated or insecure Python package versions
  - Vulnerabilities in transitive dependencies
- Artifacts: sca-reports

### DAST (Live Application with ZAP)
- Tests running application for vulnerabilities
- Runs on push to main branch
- Checks for:
  - Missing HTTP Security Headers (Content-Security-Policy, Anti-clickjacking, X-Content-Type-Options)
  - Information Leakage (Server version headers, stack traces)
  - Browser security configurations (Permissions Policy, Site Isolation)
  - Cache control misconfigurations
- Artifacts: zap-baseline-reports, zap-fullscan-reports

## Understanding Results
CodeQL (SAST) Results
Interpreting Alerts: CodeQL categorizes findings by severity (Error, Warning, Note).
High/Critical: These represent immediate security risks (e.g., the "Flask app is run in debug mode" alert seen in our scan). These must be fixed immediately as they often allow attackers to execute code or access sensitive data.
Medium/Low: These are quality or best-practice issues that should be scheduled for remediation.

SCA Results
Interpreting the Report:
Vulnerable Dependencies: Check the summary table at the top. If the count is 0, your dependencies are clean.
CVE Details: If vulnerabilities are found, scroll to the "Dependencies" section. It lists the library name, the specific CVE ID (e.g., CVE-2023-XXXX), and the severity score. These usually require upgrading the package version in requirements.txt.3

DAST Results
Interpreting the Report:
Alerts Section: This lists specific vulnerabilities found while the app was running.
Risk Levels:
Medium: (e.g., "Missing Anti-clickjacking Header", "Content Security Policy (CSP) Header Not Set"). These indicate missing security configurations that leave the app open to UI redress or XSS attacks.
Low/Informational: (e.g., "Server Leaks Version Information"). These are reconnaissance risks where the server reveals too much data about its technology stack.