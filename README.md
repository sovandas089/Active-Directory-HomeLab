# Home Lab: Splunk + Active Directory (Windows Server)

> **Purpose:** Compact lab showing AD DS on Windows Server, Splunk on Ubuntu, Windows 10 target, Kali attacker — includes diagrams and screenshot placeholders so you can drop in actual captures.

---

## 1. Cover / Executive Summary

A one-paragraph summary of the project goals and outcomes.

**Project goal:** Build an isolated lab to simulate a brute-force attack (Hydra from Kali) against a Windows 10 machine joined to an Active Directory, capture authentication and host logs in Splunk, and validate detection using Splunk dashboards and correlation rules.

**Outcome (short):** Splunk successfully ingested Windows authentication events; brute-force attempts were visible as spikes of failed logins followed by a successful authentication. Dashboards and saved searches helped quickly identify the compromise.

---

## 2. Lab Network (diagram)
<img width="793" height="922" alt="AD DS" src="https://github.com/user-attachments/assets/0c094220-71bf-443b-a96b-357c7deb3dc1" />



**Caption:** Network layout showing attacker, target, domain controller, and Splunk server.


---

## 3. Components & Roles (table)

* **Windows Server (AD DS)** — Domain Controller, user & group management, logs authentication events.
* **Ubuntu (Splunk)** — Splunk Enterprise installed; indexes Windows events forwarded from hosts.
* **Windows 10** — Domain-joined target with services and test accounts.
* **Kali Linux** — Attacker: Run Hydra for password brute-force.

---

## 4. Setup Steps (with screenshot placeholders)

1. **Windows Server: Install AD DS & promote to DC**

   * Screenshot: <img width="1688" height="885" alt="AD DS user" src="https://github.com/user-attachments/assets/4bc2b8f2-3531-461f-ac8f-c8ba00dd8ba1" />

   * Caption: Show Server Manager > AD DS promotion completed.

2. **Configure Windows 10 to join domain**

   * Screenshot: `![Joined to domain](win10_domain_placeholder.png)`

3. **Install Splunk on Ubuntu**

   * Screenshot: <img width="1686" height="885" alt="Screenshot 2025-10-04 215826" src="https://github.com/user-attachments/assets/a64e45f4-50f0-4610-bc0b-00cbdb771d12" />

   * Caption: Splunk web landing page (e.g., `http://<splunk-ip>:8000`).

4. **Forward Windows event logs to Splunk**

   * Screenshot: <img width="1919" height="875" alt="Screenshot 2025-10-04 230641" src="https://github.com/user-attachments/assets/52548d63-4593-4c1c-8195-ff9657fce1ec" />

   * Caption: Show the Universal Forwarder configuration or WinRM/WEC setup used.

5. **Prepare Kali & run Hydra**

   * Screenshot: <img width="1919" height="878" alt="Screenshot 2025-10-04 232924" src="https://github.com/user-attachments/assets/6fcf2a96-fd43-4b5e-8fc5-53b314634b5e" />

   * Caption: Hydra command and output showing brute-force attempts.

---

## 5. Attack Timeline (diagram + screenshot)

Mermaid-style timeline (or simple list):

```
1. 10:02 — Kali begins brute-force (Hydra) vs target service
2. 10:03 — Windows logs repeated failed authentication events
3. 10:07 — One credential succeeds; attacker obtains access
4. 10:09 — Domain Controller records suspicious authentication
5. 10:12 — Splunk ingests events and dashboard shows spike
```


---

## 6. Splunk Searches & Dashboards (mockups)

**Example search (high-level):**

```
index=endpoint EventCode=4625 | stats count by src_ip,dest_ip,dest_port
```

**Screenshot** 
<img width="1919" height="910" alt="Screenshot 2025-10-04 234307" src="https://github.com/user-attachments/assets/e85d9454-764d-4fd6-92a8-52565ab7b135" />


**Caption:** Dashboard panels: Failed logins over time, top source IPs, accounts with most failures, successful logins following failures.

---

## 7. Findings (short bullets)

* Splunk indexes contained clear signals: repeated failure events then success for the compromised account.
* Correlation across the Windows 10 host and the domain controller provided the timeline of compromise.
* Brute-force generated an obvious pattern that is easy to detect with simple rules.

---

## 8. Artifacts & Attachments (what to include screenshots of)

* Hydra command used + terminal output.
* Windows Event Viewer logs showing failed/successful logon Event IDs (4625, 4624, 4740 if lockouts used).
* Splunk index/ingestion confirmation and sample indexed events.
* Splunk dashboard PNG and saved search definitions.

**Placeholders:** Insert each at the locations above.

---

## 9. Remediation & Recommendations (concise)

* Enforce strong password policy and MFA.
* Implement account lockout & alerting on repeated failures.
* Restrict RDP/remote services and use VPN.
* Keep centralized logging and test detection periodically.

---

## 10. Appendix — Commands & Config Snippets (suggested)

* **Hydra example (illustrative):**

```
hydra -l administrator -P passwords.txt rdp://<target-ip>
```

* **Splunk search (illustrative high-level):**

```
index=windows EventCode=4625 | stats count by src_ip, Account_Name
```

> **Note:** These are illustrative snippets meant for documenting the lab. Do not run attacks on systems you don’t own or have explicit permission to test.




**End of document**
