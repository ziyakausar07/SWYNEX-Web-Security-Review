# Web Security Review - Recommendations & Remediation

This section outlines technical and architectural remediation measures designed to address the findings identified during the five security tests. Recommendations follow the principle of **Defense-in-Depth**.

---

## 1. Recommendations by Test Category

### Test 1: HTTP Security Headers Hardening
1. **Configure Content-Security-Policy (CSP):**
   - Define strict source directives to restrict unauthorized script execution and resource loading:
     ```http
     Content-Security-Policy: default-src 'self'; script-src 'self'; object-src 'none'; frame-ancestors 'self';
     ```
2. **Enable HTTP Strict Transport Security (HSTS):**
   - For production HTTPS deployments, prevent downgrade attacks:
     ```http
     Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
     ```
3. **Maintain Existing Headers:**
   - Retain `X-Content-Type-Options: nosniff` and configure `X-Frame-Options: SAMEORIGIN` or `DENY`.
4. **Restrict CORS:**
   - Avoid `Access-Control-Allow-Origin: *` on private API endpoints; whitelist explicitly trusted domains.

---

### Test 2: Cross-Site Scripting (XSS) Defenses
1. **Context-Aware Output Encoding:**
   - Ensure all dynamic parameters reflected into HTML elements or attributes are properly entity-encoded.
2. **Input Validation & Sanitization:**
   - Strip unsafe characters and validate search inputs against permissible alphanumeric patterns.
3. **Use Safe Framework Templating:**
   - Rely on Angular's automatic data-binding interpolation (`{{ query }}`) rather than raw DOM assignments.

---

### Test 3: SQL Injection Remediation (Urgent / Priority 1)
1. **Adopt Parameterized Queries / Prepared Statements:**
   - Eliminate direct string concatenation in SQL queries:
     ```javascript
     // Remediated Parameterized Implementation:
     models.sequelize.query(
       'SELECT * FROM Users WHERE email = :email AND password = :password',
       { replacements: { email: req.body.email, password: security.hash(req.body.password) }, type: models.sequelize.QueryTypes.SELECT }
     );
     ```
2. **Least-Privilege Database Access:**
   - Restrict database user accounts so that application accounts cannot alter tables or access administrative procedures unnecessarily.

---

### Test 4: Authentication Security Hardening
1. **Maintain Consistent Error Messages:**
   - Continue the current practice of returning generic responses like *"Invalid email or password"* to prevent username enumeration.
2. **Implement Rate Limiting & Account Lockout:**
   - Restrict repeated failed attempts (e.g., maximum 5 attempts within 10 minutes) to mitigate automated credential stuffing and brute-force attacks.
3. **Robust Password Hashing:**
   - Use industry-standard adaptive hashing algorithms such as Argon2id or bcrypt with high work factors.

---

### Test 5: Access Control & Endpoint Protection
1. **Server-Side Authorization Checks:**
   - Continue enforcing token and session verification on all REST endpoints (`/rest/user/*`, `/api/*`).
2. **Deny Unauthenticated Requests by Default:**
   - Ensure unauthenticated calls to sensitive user endpoints return 401 Unauthorized or sanitize response payloads (as observed with empty user object `{"user":{}}`).
3. **Role-Based Access Control (RBAC):**
   - Enforce distinct permission boundaries between regular customer accounts and administrator accounts.

---

## 2. Remediation Priority Roadmap

| Priority | Action Item | Target Test | Timeline |
| :---: | :--- | :---: | :---: |
| **P1 - Immediate** | Implement Parameterized SQL Queries to fix SQL Injection | Test 3 | Within 24-48 Hours |
| **P2 - Short-Term** | Configure CSP and HSTS Security Headers; Restrict CORS | Test 1 | Within 7 Days |
| **P3 - Medium-Term** | Implement Rate Limiting and Brute-Force Protection on Login | Test 4 | Next Sprint Cycle |
| **P4 - Ongoing** | Context-aware output encoding across all input vectors & continuous RBAC review | Test 2 & 5 | Continuous Integration |
