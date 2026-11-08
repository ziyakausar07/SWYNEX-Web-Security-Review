# Web Security Review - Introduction

## 1. Project Background
This web security review was conducted as part of the **SWYNEX Technologies Cybersecurity Internship (Task 3: Web Security Review)**. The primary objective is to execute a defensive security assessment of a web application within an authorized, isolated testing environment to identify vulnerabilities, assess operational risks, and formulate robust remediation strategies.

## 2. Assessment Scope & Target Application
- **Application Name:** OWASP Juice Shop (v20.3.0)
- **Application Type:** Modern Single-Page Application (SPA) built with Node.js, Express, Angular, and SQLite.
- **Testing Environment:** Localhost Isolated Lab (`http://localhost:3000` / `http://127.0.0.1:3000`)
- **Authorized Testing Boundary:** 
  - All tests were performed strictly against the local containerized/sandboxed instance.
  - In strict compliance with SWYNEX assessment rules and ethical guidelines, **no external or third-party production infrastructure was targeted**.

## 3. Assessment Objectives
1. **Identify Common Web Vulnerabilities:** Perform targeted tests mapped to the **OWASP Top 10** web application security risks.
2. **Defensive Evaluation:** Analyze the effectiveness of existing application-layer defenses, headers, and access controls.
3. **Risk & Impact Analysis:** Categorize identified findings using standard risk metrics (Likelihood, Business Impact, Severity).
4. **Actionable Remediation:** Provide defense-in-depth recommendations to harden the application against prospective exploitation.

## 4. Assessment Summary Matrix
| Metric | Details |
| :--- | :--- |
| **Intern Name** | Ziyakausar Tamboli |
| **Intern ID** | SWX-2026-002678 |
| **Program** | Cyber Security Internship |
| **Organization** | SWYNEX Technologies |
| **Task Reference** | Task 3: Defensive Web Security Review |
| **Date of Review** | November 2026 |
| **Target Application** | OWASP Juice Shop v20.3.0 |

