# SWYNEX Technologies - Cybersecurity Internship
## Task 3: Defensive Web Security Review

[![Cybersecurity Internship](https://img.shields.io/badge/SWYNEX-Cybersecurity%20Internship-007ACC?style=for-the-badge&logo=shield)](https://swynex.com)
[![Task Status](https://img.shields.io/badge/Task%203-Complete%20%2F%20Ready%20for%20Review-28A745?style=for-the-badge&logo=github)](https://github.com)
[![Target](https://img.shields.io/badge/Target-OWASP%20Juice%20Shop-orange?style=for-the-badge)](http://localhost:3000)

---

## 📌 Intern & Project Information
- **Intern Name:** Ziyakausar Tamboli
- **Intern ID:** `SWX-2026-002678`
- **Program:** Cyber Security Internship (30 Days)
- **Organization:** [SWYNEX Technologies](https://swynex.com)
- **Task Title:** Task 3 - Web Security Review
- **Testing Scope:** Localhost Authorized Lab (`http://localhost:3000` / `127.0.0.1:3000`)

---

## 📖 Executive Summary
This repository contains the deliverables, findings, risk assessment, and technical remediation documentation for **Task 3: Web Security Review**.

The objective of this assessment was to perform a comprehensive, defensive web security evaluation of a target single-page web application (**OWASP Juice Shop**) running inside an isolated local testing environment. In strict adherence to SWYNEX guidelines, all activities were restricted to the authorized local instance, ensuring zero disruption or scanning of third-party systems.

Five primary security tests were executed:
1. **Test 1: Security Misconfiguration – HTTP Security Headers Review**
2. **Test 2: Cross-Site Scripting (XSS) Testing**
3. **Test 3: SQL Injection – Authentication Bypass**
4. **Test 4: Authentication Security Testing**
5. **Test 5: Access Control Testing**

---

## 🗂️ Repository Structure

```text
SWYNEX-Web-Security-Review/
│
├── README.md                                      # Primary project overview and summary
├── Final_Report.pdf                               # Compiled assessment report document
│
├── Documentation/                                 # Detailed technical reports
│   ├── Introduction.md                            # Background, scope, environment details
│   ├── Methodology.md                             # Testing methodology & testing phases
│   ├── Security_Findings.md                       # In-depth breakdown of the 5 security tests
│   ├── Risk_Assessment.md                         # CVSS scoring, risk matrix & business impact
│   └── Recommendations.md                         # Hardening strategies & remediation roadmap
│
└── Screenshots/                                   # Evidence & testing verification screenshots
    ├── 01_Security_Misconfiguration.png           # Test 1: HTTP security response headers
    ├── 02_XSS_Testing.png                         # Test 2: Search input payload handling
    ├── 03A_SQL_Injection_Payload.png              # Test 3: SQLi payload input in login form
    ├── 03B_SQL_Injection_Success.png              # Test 3: Solved challenge & admin bypass
    ├── 04A_Authentication_Registration.png         # Test 4: User registration baseline
    ├── 04B_Authentication_Invalid_Credentials.png  # Test 4: Generic error message verification
    ├── 05A_Access_Control_LoggedIn.png            # Test 5: /rest/user/whoami authenticated
    └── 05B_Access_Control_LoggedOut.png           # Test 5: /rest/user/whoami unauthenticated
```

---

## 🔍 Security Tests & Findings Summary

| Test ID | Test Name | Target Component | Observed Status | Severity |
| :---: | :--- | :--- | :--- | :---: |
| **01** | Security Misconfiguration – HTTP Headers | Response Headers | 2 headers present, CSP & HSTS absent | Preliminary (Medium) |
| **02** | Cross-Site Scripting (XSS) Testing | Product Search | Encoded in URL, no script execution in screenshot | Not Confirmed |
| **03** | SQL Injection – Authentication Bypass | Login Form | Admin authentication bypassed via `' OR 1=1--` | **High** |
| **04** | Authentication Security Testing | Login Verification | Displays safe "Invalid email or password" error | Low / Informational |
| **05** | Access Control Testing | `/rest/user/whoami` | User data protected when logged out (`{"user":{}}`) | Low / Verified Secure |

---

## 🛡️ Key Recommendations Summary
1. **Parameterized Queries:** Use prepared statements and ORM replacements to eliminate SQL Injection on authentication endpoints.
2. **Deploy Security Headers:** Configure a robust `Content-Security-Policy` (CSP) and enforce `Strict-Transport-Security` (HSTS).
3. **Output Encoding:** Implement context-aware HTML entity encoding on all user inputs.
4. **Authentication Controls:** Maintain consistent error messages and implement rate limiting / account lockout mechanisms against brute force.
5. **Access Control Enforcement:** Continue validating session tokens server-side before delivering user profile data.

---

## 📚 Detailed Documentation Links
- [Introduction & Scope](Documentation/Introduction.md)
- [Assessment Methodology](Documentation/Methodology.md)
- [Complete Security Findings](Documentation/Security_Findings.md)
- [Risk Assessment & Analysis](Documentation/Risk_Assessment.md)
- [Recommendations & Remediation Roadmap](Documentation/Recommendations.md)

---

## ⚖️ Ethics & Compliance Disclaimer
This assessment was performed strictly for educational and defensive evaluation purposes within an isolated local lab environment authorized by SWYNEX Technologies. No production, third-party, or external infrastructure was assessed or targeted.
