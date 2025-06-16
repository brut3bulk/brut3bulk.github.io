# IoT/Edge Device Pentest Report (Anonymized)

## Summary

A grey-box security assessment was conducted on an industrial IoT edge device responsible for processing sensor data and communicating with upstream systems. The device included local API endpoints, networking functions, and embedded credential storage. Several critical vulnerabilities were identified across different layers of the system.

---

## 🔍 Findings

### 1. ARP Spoofing / Poisoning

**Description:**  
The device accepted ARP responses without verification, allowing ARP cache poisoning. An attacker was able to intercept traffic via Man-in-the-Middle (MitM) techniques.

**Impact:**  
- Traffic interception and manipulation  
- Credential/session theft  
- Data leakage across the internal network

**Recommendation:**  
- Use static ARP entries where feasible  
- Implement ARP inspection and encrypted protocols

---

### 2. DDoS Vulnerability Across Network Layers

**Description:**  
The device was susceptible to Distributed Denial-of-Service (DDoS) attacks on multiple layers:

- **Layer 3 (Volumetric):** Flooding the device with raw traffic exceeded interface capacity.  
- **Layer 4 (Protocol):** TCP SYN floods left sockets in half-open state, exhausting system resources.  
- **Layer 7 (Application):** HTTP GET/POST floods overwhelmed web service logic and exhausted memory under concurrent load.  
- **TLS Exhaustion:** The TLS handshake process was abused by repeatedly initiating SSL connections without completion, consuming CPU and memory due to expensive cryptographic operations.

**Impact:**  
- Service unavailability in critical industrial processes  
- Latency spikes and dropped telemetry packets  
- Device instability and loss of encrypted services (TLS downtime)

**Recommendation:**  
- Deploy edge-layer DDoS mitigation (rate limiting, connection timeouts)  
- Use SYN cookies, HTTP challenge mechanisms, and TLS rate limiting  
- Monitor SSL handshake patterns and throttle excessive concurrent sessions

---

### 3. Weak Password Hashing (MD5)

**Description:**  
User credentials were stored using the outdated MD5 hashing algorithm, which is susceptible to brute-force and collision attacks.

**Impact:**  
- Offline recovery of passwords  
- Full device compromise if credentials reused  
- Elevated risk of lateral movement

**Recommendation:**  
- Replace MD5 with bcrypt or SHA-256 with salt  
- Enforce strong password policies  
- Use key stretching and peppering

---

## Engagement Details

- **Mode:** Grey-Box Testing  
- **Scope:** Industrial IoT/Edge gateway device  
- **Duration:** Confidential  
- **Environment:** Real-world industrial deployment with connected sensors and remote server interaction

---

> **Note:** This report is anonymized. All findings represent real vulnerabilities discovered under ethical guidelines in an undisclosed environment. No vendor or device identifiers are included.
