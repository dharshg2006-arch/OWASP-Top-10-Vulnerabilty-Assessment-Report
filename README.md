# OWASP-Top-10-Vulnerabilty-Assessment-Report

## 1. Executive Summary
This security assessment evaluates the authentication and input-handling posture of the target enterprise web application dashboard. The assessment was executed against a live Splunk Enterprise portal to verify defensive measures against common OWASP Top 10 vulnerabilities.

---

## 2. SQL Injection (SQLi) Assessment
* **Vulnerability Type:** OWASP A03:2021 - Injection
* **Target Endpoint:** `http://10.81.28`
* **Test Payload:** `admin' OR 1=1 --`

### Assessment & Proof of Concept (PoC)
An authentication bypass attempt was performed on the primary entry form. The application handled the malicious string securely, returning a generic error page and blocking query logic changes. This indicates strong server-side controls.


### Remediation Code (Defensive Best Practice)
To maintain this posture, developers must ensure input handling continues to rely on strictly parameterized queries rather than dynamic concatenation:
```php
// Example of secure parameterized execution
stmt = conn->prepare("SELECT user, pass_hash FROM accounts WHERE user = ?");
\$stmt->bind_param("s", \(inputUsername);\)stmt->execute();
```

---

## 3. Cross-Site Scripting (XSS) Mitigation Analysis
* **Vulnerability Type:** OWASP A03:2021 - Injection (XSS)

### Assessment Observation
The application interface applies structural context encoding across dynamic URL parameters (e.g., handling variables like `return_to=%2Fen-US%2F` safely). This prevents the injection of malicious reflection scripts.

### Remediation Code (Defensive Best Practice)
Web applications should always perform context-aware encoding before outputting variables back to the browser:
```php
echo htmlspecialchars(\$user_input_string, ENT_QUOTES, 'UTF-8');
```
