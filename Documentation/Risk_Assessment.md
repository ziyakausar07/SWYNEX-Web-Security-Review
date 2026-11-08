# Web Security Review - Risk Assessment

## 1. Risk Evaluation Framework
Vulnerabilities and security observations identified during this defensive assessment are evaluated using the standard Risk Assessment Matrix (Likelihood vs. Impact) and the **Common Vulnerability Scoring System (CVSS v3.1)**.

### Risk Matrix:
| Likelihood \ Impact | Low | Medium | High | Critical |
| :--- | :---: | :---: | :---: | :---: |
| **High** | Medium | High | Critical | **Critical** |
| **Medium** | Low | Medium | High | **Critical** |
| **Low** | Low | Low | Medium | **High** |

---

## 2. Test-by-Test Risk & Severity Analysis

| Test ID | Test Name | Likelihood | Impact | Severity Rating | Status / Justification |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **Test 1** | Security Misconfiguration – HTTP Security Headers | Medium | Low | **Preliminary (Medium)** | Missing CSP & HSTS increases exposure to client-side attacks, but framing controls (`SAMEORIGIN`, `nosniff`) are active. |
| **Test 2** | Cross-Site Scripting (XSS) Testing | Low | High | **Not Confirmed** | No execution observed during this search payload test. Requires deeper testing across other input vectors. |
| **Test 3** | SQL Injection – Authentication Bypass | High | Critical | **High / Critical** | Critical flaw: `' OR 1=1--` bypasses login completely and provides full administrative access without credentials. |
| **Test 4** | Authentication Security – Invalid Login Handling | Low | Low | **Low / Informational** | Secure behavior observed: Application provides generic "Invalid email or password" error, preventing user enumeration. |
| **Test 5** | Access Control – Endpoint Authorization | Low | Low | **Low / Verified Secure** | Secure behavior observed: `/rest/user/whoami` returns `{"user":{}}` when session is terminated, withholding sensitive identity info. |

---

## 3. High-Priority Risk Details

### Test 3: SQL Injection Authentication Bypass (Severity: High / Critical)
- **Vector Description:** The application concatenates unsanitized email strings into SQL statements.
- **Exploitation Likelihood:** High (can be executed directly via browser without specialized tools).
- **Impact:** Critical (complete compromise of confidentiality, integrity, and administrative takeover).
- **Action Required:** Immediate migration to parameterized queries and prepared statements.
