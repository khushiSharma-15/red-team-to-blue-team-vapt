Red Team to Blue Team: Web Application VAPT with a SIEM Roadmap

An end-to-end, hands-on cybersecurity lab project covering both offensive and defensive security on a deliberately vulnerable web application.

Overview

This project documents a Web Application Vulnerability Assessment and Penetration Test (VAPT) performed against DVWA (Damn Vulnerable Web App) in an isolated local lab (Kali Linux attacker VM + DVWA target VM), followed by a planned Blue Team / SIEM phase to detect the same attacks via log monitoring and threat hunting.

The goal is to show the full security lifecycle:

Attack a vulnerable application as a red teamer, using an industry-standard methodology.
Document each finding with evidence, impact, and remediation.
Defend by feeding the same attack traffic into a SIEM and building detections for it (future scope).

⚠️ All testing was performed exclusively against DVWA running in a private, isolated virtual lab environment. No real or production systems were targeted. This project is for educational purposes only.

Methodology

A structured, four-stage VAPT methodology (aligned with PTES / OWASP Testing Guide phases) was applied consistently across every vulnerability class:

Reconnaissance & Mapping — Port/service enumeration (Nmap) and full application mapping (Burp Suite HTTP History).
Vulnerability Identification — Safe test payloads to confirm whether a vulnerability class is actually present.
Exploitation — Manual exploitation first to build genuine understanding, with repetitive extraction automated via Burp Intruder where applicable.
Reporting & Remediation — Each finding documented with evidence, real-world impact, and a concrete fix.
Tools Used
Category	Tools
Reconnaissance	Nmap
Web proxy / exploitation	Burp Suite (Proxy, Repeater, Intruder, Decoder, Comparer, Sequencer)
Password cracking	John the Ripper, Hashcat
Target application	DVWA (Damn Vulnerable Web App)
Attacker OS	Kali Linux
Planned (Blue Team)	Wazuh, Filebeat, Sysmon/auditd, Kibana, MITRE ATT&CK
Vulnerabilities Covered
SQL Injection — in-band (UNION-based) database enumeration and data extraction, plus boolean-based and time-based blind SQL injection (including Burp Intruder–assisted automation).
Cross-Site Scripting (XSS) — reflected, stored, and DOM-based, including filter-bypass techniques and an overview of the different injection contexts (HTML body, attributes, event handlers, URL/JS contexts, DOM sinks).
Broken Authentication — offline cracking of extracted password hashes.
Authorization Bypass / Broken Access Control (IDOR) — accessing admin-only functionality as a low-privileged user.
Findings Summary (mapped to OWASP Top 10)
Finding	OWASP Category	Severity
SQL Injection – in-band (UNION-based)	Injection	Critical
SQL Injection – blind (boolean & time-based)	Injection	Critical
Weak password hashing (unsalted)	Cryptographic Failures	High
XSS – Reflected	Injection (XSS)	High
XSS – Stored	Injection (XSS)	Critical
XSS – DOM-based	Injection (XSS)	High
Authorization Bypass / Broken Access Control	Broken Access Control	High

Severity ratings are illustrative, based on general OWASP guidance for these vulnerability classes in a typical production context — DVWA itself is a deliberately vulnerable lab application, not a production system.

Remediation Highlights
SQL Injection — parameterized queries/prepared statements, least-privilege DB accounts, server-side input validation, generic error messages, query timeouts, and a WAF as defense-in-depth (not a substitute for fixing the code).
XSS — context-aware output encoding, Content Security Policy (CSP), allow-list sanitization (e.g. DOMPurify or framework auto-escaping) instead of denylist filtering, HttpOnly/Secure/SameSite cookie flags, and avoiding unsafe DOM sinks (innerHTML, document.write, eval).
Authorization Bypass — server-side enforcement on every request, a formal authorization matrix (role × resource × action), object-level ownership checks (anti-IDOR), consistent enforcement across UI and API, deny-by-default access, and logging/alerting on repeated authorization failures.
Post-Test Cleanup

All testing was performed inside an isolated VM lab. The DVWA database was reset via its built-in "Setup / Reset DB" option after the assessment, and no real systems were affected.

Future Scope — Blue Team / SIEM Extension

This project currently demonstrates the offensive (red team) half of the security lifecycle. The planned next phase closes the loop with a defensive (blue team) capability in the same lab:

Forward DVWA/web server and Burp-generated attack traffic into a SIEM (e.g. Wazuh, Splunk, or the ELK stack).
Build detection rules for each attack class demonstrated here (e.g. anomalous response-time variance for blind SQLi, suspicious injection patterns in submitted fields, requests to admin-only endpoints from non-admin sessions).
Perform real-time log analysis and threat hunting to validate detection coverage rather than relying on alerts alone.
Report a measurable detection rate per attack class, closing the loop between "I can exploit this" and "I can detect this being exploited."
Repository Contents
Red_Team_to_Blue_Team.pptx — Full project write-up and walkthrough (slides).

Link-https://docs.google.com/presentation/d/1KcqxmePPnZUeuedY4EvxTWavi8dkJuZM/edit?pli=1&slide=id.g38bafc61219_0_69#slide=id.g38bafc61219_0_69
Disclaimer

This project was conducted entirely in a personal, isolated lab environment against intentionally vulnerable software (DVWA) for educational purposes. It does not target, and was never used against, any real or production system. Always obtain explicit written authorization before testing any system you do not own.

