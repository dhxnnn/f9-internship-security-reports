# F9 Internship Security Reports

Two reports from my cybersecurity internship at F9 Infotech (July to September 2026):

1. **API Security Testing Master Report**: a hands-on penetration test of an intentionally vulnerable API, mapped to the OWASP API Security Top 10 (2023).
2. **Sysmon Threat Hunting Report**: an endpoint threat-hunting exercise on a Windows machine, mapped to MITRE ATT&CK.

All testing was done on intentionally vulnerable lab applications (OWASP crAPI and OWASP Juice Shop) and on my own lab machine. No real systems, customers or data were tested. Identifying details (supervisor name, machine hostname and IP) have been redacted.

---

## 1. API Security Testing (VAPT)

**Targets:** OWASP crAPI, OWASP Juice Shop
**Primary tool:** Burp Suite Community Edition (with Postman, Docker, Kali Linux, jwt.io)
**Framework:** OWASP API Security Top 10 (2023)

The Master Report is the full day-by-day record. It contains 44 documented test cases with request and response evidence, plus three formal deliverables inside the same document: a **VAPT Report** (executive summary, technical findings, risk ratings), an **API Security Checklist** and a **Remediation Tracker**.

### Confirmed findings

| ID | Finding | OWASP category | Severity |
|----|---------|----------------|----------|
| F-01 | JWT signature verification bypass (`alg:none`), full account impersonation | API2 Broken Authentication | Critical |
| F-02 | Cross-user vehicle location access by swapping a vehicle ID | API1 BOLA | Critical |
| F-03 | Unauthorised admin video deletion by a regular user (undocumented endpoint found via the OpenAPI spec) | API5 BFLA / API9 Improper Inventory Management | Critical |
| F-04 | NoSQL injection on coupon validation, extracting a valid coupon with real balance impact | API6 Sensitive Business Flows | High |
| F-05 | Mechanic report enumeration by sequential ID | API1 BOLA / IDOR | High |
| F-06 | Server-side request forgery via the mechanic contact API | API7 SSRF | High |
| F-07 | Session not invalidated on logout | API2 Broken Authentication | Medium |
| F-08 | Open CORS policy and missing CSP | API8 Security Misconfiguration | Medium |
| F-09 | Unsafe consumption of upstream API responses | API10 Unsafe Consumption of APIs | Medium |
| F-10 | No rate limiting on login or OTP generation (15 and 20 repeated requests accepted, no lockout or CAPTCHA) | API4 Unrestricted Resource Consumption | High |

**Common theme:** the application checks that you are logged in, but does not consistently check that you are allowed to access the specific object or function you asked for. Fixing authorisation at framework level would close most of the Critical and High findings at once.

---

## 2. Sysmon Threat Hunting

**Goal:** instrument a Windows endpoint with Sysmon, generate representative telemetry through controlled, benign test activity, and hunt through the logs as a SOC analyst would, without relying on pre-built alerts.

**Setup:** Sysmon with a hardened, threat-hunting-oriented configuration (SwiftOnSecurity baseline, extended).

**Hunting areas covered**
- Encoded and obfuscated PowerShell execution
- Living-off-the-Land Binaries (LOLBins), including `mshta.exe`
- Suspicious parent-child process relationships
- Persistence: registry Run key and scheduled task
- Outbound network connections initiated by processes

Each finding includes the Sysmon event IDs, indicators of compromise, MITRE ATT&CK mapping (including T1059.001, T1027, T1218, T1547.001, T1053.005, T1071) and remediation or detection recommendations.

**Note:** the telemetry came from controlled test activity on my own machine, designed to mimic the techniques in scope. It is a training exercise, not a real incident.

---

## Files

| File | Description |
|------|-------------|
| `reports/API_Security_Testing_Master_Report.pdf` | Full API VAPT record, including VAPT report, checklist and remediation tracker |
| `reports/Sysmon_Threat_Hunting_Report.pdf` | Sysmon endpoint threat-hunting investigation |

## Author

Dhanush Santhosh Kumar. BSc Cybersecurity Engineering (University of West London, RAK), Certified Ethical Hacker (EC-Council).
GitHub: [dhxnnn](https://github.com/dhxnnn)
