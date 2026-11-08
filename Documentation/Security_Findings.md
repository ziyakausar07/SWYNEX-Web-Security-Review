# Web Security Review - Security Findings

This document presents the detailed observations, test methodologies, security impact, recommendations, and evidence collected across the five security tests performed on the target application (**OWASP Juice Shop**).

---

## Findings Summary Matrix

| Test ID | Test Name | Target Component | Observed Status | Severity | Evidence Files |
| :---: | :--- | :--- | :--- | :---: | :--- |
| **Test 1** | Security Misconfiguration – HTTP Security Headers Review | HTTP Response Headers | 2 headers present, CSP & HSTS not visible | Preliminary (Medium) | `Screenshots/01_Security_Misconfiguration.png` |
| **Test 2** | Cross-Site Scripting (XSS) Testing | Product Search Input | Encoded in URL, no script execution in screenshot | Not Confirmed | `Screenshots/02_XSS_Testing.png` |
| **Test 3** | SQL Injection – Authentication Bypass | Login Form | Successfully solved "Login Admin" challenge | **High** | `Screenshots/03A_SQL_Injection_Payload.png`<br>`Screenshots/03B_SQL_Injection_Success.png` |
| **Test 4** | Authentication Security Testing | Login Credential Validation | Returns generic "Invalid email or password" error | Low / Informational | `Screenshots/04A_Authentication_Registration.png`<br>`Screenshots/04B_Authentication_Invalid_Credentials.png` |
| **Test 5** | Access Control Testing | `/rest/user/whoami` Endpoint | Sensitive user data protected when logged out | Low / Verified Secure | `Screenshots/05A_Access_Control_LoggedIn.png`<br>`Screenshots/05B_Access_Control_LoggedOut.png` |

---

## Test 1: Security Misconfiguration – HTTP Security Headers Review

- **Application:** OWASP Juice Shop
- **Testing Environment:** Localhost (127.0.0.1 / localhost:3000)
- **Objective:** To review HTTP response headers and identify security configuration observations.

### Observations
1. `X-Content-Type-Options`: Present (`nosniff`).
2. `X-Frame-Options`: Present (`SAMEORIGIN`).
3. `Content-Security-Policy`: Not visible in the captured portion of response headers.
4. `Strict-Transport-Security`: Not visible in the captured portion of response headers.
5. `Access-Control-Allow-Origin`: Present (`*`).

### Security Impact
Missing or improperly configured security headers may increase exposure to certain browser-based attacks, depending on the application and deployment configuration. In the absence of CSP, any potential script execution cannot be constrained by browser policy.

### Recommended Remediation
- Configure an appropriate Content-Security-Policy (CSP).
- Enable HSTS on production HTTPS deployments.
- Retain `X-Content-Type-Options: nosniff`.
- Configure an appropriate `X-Frame-Options` or CSP `frame-ancestors` policy.
- Restrict `Access-Control-Allow-Origin` from wildcard to trusted origins.

### Preliminary Result
Two security headers were observed (`X-Content-Type-Options`, `X-Frame-Options`). Further inspection is required to verify the remaining headers before assigning a final vulnerability severity.

- **Evidence:** `Screenshots/01_Security_Misconfiguration.png`

---

## Test 2: Cross-Site Scripting (XSS) Testing

- **Application:** OWASP Juice Shop
- **Testing Environment:** Localhost:3000
- **Objective:** To check whether the application safely handles user-supplied search input.

### Test Payload
`<script>alert('XSS Test')</script>`

### Observations
- Entered the test payload into the search field.
- The browser displayed the input in encoded form in the URL (`/#/search?q=<script>alert('XSS%20Test')<%2Fscript>`).
- The application displayed *"No results found"*.
- No JavaScript alert execution was visible in the provided screenshot.

### Result
No XSS execution was observed during this particular search-input test.

### Recommended Remediation
- Apply context-aware output encoding across all user inputs.
- Validate and sanitize untrusted user input.
- Implement an appropriate Content Security Policy (CSP).
- Perform additional testing on other input locations (feedback form, user profile fields, etc.).

- **Severity:** Not confirmed.
- **Evidence:** `Screenshots/02_XSS_Testing.png`

---

## Test 3: SQL Injection – Authentication Bypass

- **Application:** OWASP Juice Shop
- **Environment:** Localhost:3000
- **Objective:** To evaluate whether unsafe SQL query handling could allow unauthorized authentication.

### Testing Method
Tested the login form using a SQL injection test input (`' OR 1=1--`) in the authorized OWASP Juice Shop training environment.

### Observed Result
- Submitting `' OR 1=1--` into the email field allowed authentication without a valid password.
- The application displayed the successful *"Login Admin"* challenge notification: *"You successfully solved a challenge: Login Admin (Log in with the administrator's user account.)"*.
- Administrator authentication was successfully obtained.
- This demonstrates an authentication bypass in the intentionally vulnerable training application.

### Security Impact
An attacker exploiting SQL injection may bypass authentication and gain unauthorized access to sensitive application functionality and administrative control.

### Severity
**High**

### Recommended Remediation
1. Use parameterized SQL queries and prepared statements.
2. Avoid constructing SQL queries through direct string concatenation.
3. Apply server-side input validation.
4. Use least-privilege database accounts.
5. Perform regular security testing and code review.

### Conclusion
The SQL injection challenge was successfully demonstrated in the authorized local training environment. The result highlights the importance of secure database query handling.

- **Evidence:** 
  - `Screenshots/03A_SQL_Injection_Payload.png` (Test payload input in login form)
  - `Screenshots/03B_SQL_Injection_Success.png` (Challenge solved / Admin authentication banner)

---

## Test 4: Authentication Security Testing

- **Application:** OWASP Juice Shop
- **Environment:** Localhost:3000
- **Objective:** To evaluate the application's handling of invalid login credentials.

### Testing Method
1. Registered test user account (`abc@gmail.com`).
2. Opened the Juice Shop login page.
3. Entered the email address (`abc@gmail.com`).
4. Entered an incorrect password.
5. Clicked the Login button.
6. Observed the application's response.

### Expected Secure Behaviour
The application should reject invalid credentials and prevent unauthorized access without disclosing which component (email vs password) was incorrect.

### Result
- The application properly rejected the login attempt and displayed a generic, secure error message:
  **"Invalid email or password."**
- This prevents user enumeration via error message timing or content discrepancies.

### Recommended Remediation
- Maintain secure password verification routines.
- Use strong password hashing (e.g., Argon2, bcrypt).
- Implement rate limiting / account lockout mechanisms against repeated login attempts.
- Maintain safe, consistent authentication error messages.
- Protect user sessions with secure cookie flags (`HttpOnly`, `Secure`, `SameSite`).

- **Severity:** Not confirmed / Low (Observed behaviour follows secure error handling design).
- **Evidence:**
  - `Screenshots/04A_Authentication_Registration.png` (User registration baseline)
  - `Screenshots/04B_Authentication_Invalid_Credentials.png` (Invalid credentials error message response)

---

## Test 5: Access Control Testing

- **Application:** OWASP Juice Shop
- **Environment:** Localhost:3000
- **Objective:** To verify whether unauthorized users can access protected account information.

### Testing Method
1. Logged into the application using the test account (`abc@gmail.com`).
2. Opened the `/rest/user/whoami` endpoint.
3. Observed the authenticated user's response:
   ```json
   {
     "user": {
       "id": 25,
       "email": "abc@gmail.com",
       "lastLoginIp": "0.0.0.0",
       "profileImage": "/assets/public/images/uploads/default.svg"
     }
   }
   ```
4. Logged out of the application.
5. Accessed the same endpoint again without authentication.
6. Observed the unauthenticated response:
   ```json
   {
     "user": {}
   }
   ```
7. Compared both responses.

### Expected Secure Behaviour
The application should not disclose authenticated user information to users without a valid session.

### Result
- When authenticated, `/rest/user/whoami` returns the identity of the currently active session.
- When unauthenticated / logged out, the endpoint correctly returns an empty user object (`{"user":{}}`), withholding sensitive personal identity details.

### Recommended Remediation
- Maintain robust server-side authentication checks on all API endpoints.
- Protect sensitive API endpoints with token validation middleware.
- Apply role-based access control (RBAC) where required.
- Continue ensuring personal information is never exposed to unauthorized users.

- **Severity:** Verified Secure / Low Risk for this specific check.
- **Evidence:**
  - `Screenshots/05A_Access_Control_LoggedIn.png` (Authenticated response displaying user profile data)
  - `Screenshots/05B_Access_Control_LoggedOut.png` (Logged-out response showing empty user object)
