# 🛡️ Threat Hunting Scenario — Northstar Financials  

## 📖 Overview  

<img width="600" height="600" alt="62ec0fb6-af5e-48fb-a78f-c18a1e26aefa" src="https://github.com/user-attachments/assets/e0848e06-0ce2-4801-999e-9d64967976ee" />

This project simulates an adversary targeting **Northstar Financials**, a mid-sized accounting firm. An employee workstation (**WIN11-FIN01**) was attacked from a **test system (Kali Linux)** using common adversary techniques.  

The goal: demonstrate how the SOC pipeline (Defender for Endpoint → Security Onion → TheHive) detects and responds to credential access and persistence attempts.  

---

## 👥 Scenario Narrative  
On **August 29th, 2025**, a test system attempted to brute force the **Administrator** account on WIN11-FIN01. Following access attempts, the adversary simulated **credential dumping** and **registry persistence** using Atomic Red Team.  

Microsoft Defender for Endpoint **blocked and alerted** on all malicious actions — producing the exact telemetry needed to validate SOC visibility.  

---

## 🎯 Scope  
**In Scope:**  
- Brute force simulation (T1110.003)  
- LSASS credential dumping simulation (T1003.001)  
- Registry persistence simulation (T1547.001)  
- Detections in Microsoft Defender for Endpoint  

**Out of Scope:**  
- Actual credential compromise  
- Sentinel hunting queries (no data, blocked by Defender)  
- Lateral movement beyond single host  

---

## ⚔️ Attack Simulation  

### Step 1 — Initial Access (Brute Force – T1110.003)  
- Tool: **Hydra** from Kali  
- Target: `192.168.60.51` (Win11) SMB service  
- Outcome: Multiple failed logons recorded in Defender 

<img width="1033" height="746" alt="vmwarelist" src="https://github.com/user-attachments/assets/57777d76-fcff-49ee-9752-7e2e13666621" />

<img width="1660" height="811" alt="Screenshot 2025-08-29 135241" src="https://github.com/user-attachments/assets/de63cf19-8295-495b-9f02-3ba71ce4fe69" />


---

### Step 2 — Credential Access (LSASS Dump – T1003.001)  
- Tool: **Atomic Red Team**  
- Action: Invoke-AtomicTest T1003.001 (dump `lsass.exe`)  
- Outcome: Defender **blocked execution**, flagged `HackTool:Win32/DumpLsass.A`  

<img width="960" height="506" alt="Screenshot 2025-08-29 135653" src="https://github.com/user-attachments/assets/4358ae42-6d33-46ed-b3d3-8b0f28b5175c" />


<img width="1703" height="832" alt="Screenshot 2025-08-29 135909" src="https://github.com/user-attachments/assets/c251c79f-51de-4e3d-ae56-9d3a23932542" />

---

### Step 3 — Persistence (Registry Run Keys – T1547.001)  
- Tool: **Atomic Red Team**  
- Action: Invoke-AtomicTest T1547.001 (add Run key)  
- Outcome: Defender flagged **Registry Run Key / Startup Folder** persistence attempt  

<img width="957" height="504" alt="Screenshot 2025-08-29 140129" src="https://github.com/user-attachments/assets/ff870a25-b75a-431b-b664-ab113d8e8b09" />


<img width="1711" height="838" alt="Screenshot 2025-08-29 140341" src="https://github.com/user-attachments/assets/58e1a19e-823c-4b25-9ac9-207f24602e26" />
 

---

## 🏁 Conclusion  
This lab successfully demonstrated:  
- How **attack simulation tools** (Hydra, Atomic Red Team) generate realistic adversary activity.  
- How **Microsoft Defender for Endpoint** detects and blocks credential access and persistence attempts.  
- How alerts are correlated to **MITRE ATT&CK techniques** (T1110.003, T1003.001, T1547.001).  

Even though Sentinel queries did not populate (because Defender blocked execution), the project proves end-to-end visibility and detection capability — the same workflow analysts would follow in a real SOC.  
