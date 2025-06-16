# SCADA Application Pentest Report (Anonymized)

## Summary

A targeted white-box penetration test was conducted on a SCADA (Supervisory Control and Data Acquisition) application integrated with an industrial control system. The system utilized PLCs and a web-based HMI interface to monitor and control remote physical processes. Multiple critical vulnerabilities were discovered during the assessment.

---

## 🔍 Findings

### 1. Cross-Site Scripting (XSS)

**Description:**  
The SCADA HMI web interface was vulnerable to both stored and reflected XSS attacks. User inputs were rendered without proper encoding, allowing malicious JavaScript injection.

**Impact:**  
- Session hijacking  
- Token theft  
- Arbitrary code execution in the victim’s browser

**Recommendation:**  
- Apply strict input validation and context-aware output encoding  
- Use CSP (Content Security Policy) headers

---

### 2. Privilege Escalation

**Description:**  
Improper session management and weak access controls allowed vertical privilege escalation from regular user to administrative access.

**Impact:**  
- Complete control over the SCADA application  
- Unauthorized access to sensitive functions and data  
- Possible modification of control logic

**Recommendation:**  
- Enforce role-based access control (RBAC)  
- Harden session handling and separate user/admin contexts

---

### 3. Insecure JWT Configuration

**Description:**  
JWT (JSON Web Tokens) issued after login had excessive expiration times, making them vulnerable to token leakage and reuse.

**Impact:**  
- Long-lived access even if user logs out  
- Elevated risk if token is intercepted

**Recommendation:**  
- Use short-lived tokens and implement refresh tokens  
- Rotate keys and enforce token revocation policies

---

## Engagement Details

- **Mode:** White-Box Testing  
- **Scope:** Web interface of SCADA HMI + authentication components  
- **Duration:** Confidential  
- **System:** Industrial monitoring and control, PLC-connected interface

---

> **Note:** This report is anonymized. No identifiable vendor, client, or facility data is included. Vulnerabilities shown reflect real-world conditions observed in industrial environments.
