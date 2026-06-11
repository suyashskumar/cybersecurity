# SOC Fundamentals

**SOC**: Security Operations Centre

**Two Responsibilities**: 
1. Detect
2. Response

**Three Pillars**: 
1. People
2. Process
3. Technology

### People

(1) CISO<br>
(2) SOC Manager<br>
(3a-c) SOC Analyst (Level 1-3)<br>
(3d) Security Engineer<br>
(3e) Detection Engineer<br>

| Role | Explanation |
| :--- | :--- |
| SOC Analyst (Level 1) | First responders to detection, perform basic alert triage to determine harmfulness of detected vulnerability |
| SOC Analyst (Level 2) | Deep-dive investigations and further analysis |
| SOC Analyst (Level 3) | Support incident response, responsible for containment, eradication, and recovery of severe threats marked by Level 1 and 2 |
| Security Engineer | Deploy and configure security solutions |
| Detection Engineer | Make security rules |
| SOC Manager | Communicate activities to CISO and manage the rest of the roles |

### Process

(1) **Alert Triage**: Answer the 5 Ws (Who, What, When, Where, Why) <br>
(2) **Reporting**: Use proper channels to communicate aforementioned findings <br>
(3) **DFIR (Digital Forensics & Incident Response)**: Advanced teams for more malicious threats <br>

### Technology

(1) SIEM: **Security Information and Event Management** (SIEM) is a popular tool for collecting logs. Modern SIEMs surpass rule-based detection and provide us with behavioral analytics and threat intelligence capability, enhanced by machine learning. **Only supports detection**, not response. <br>
(2) EDR: **Endpoint Detection and Response** (EDR) operates on the endpoint level, provides real-time and historical visibility of device activity. **Can detect and respond** in a few clicks. <br>
(3) **Firewall**: Network security, filtering unauthorized network access. <br>
(4) Others: **Antivirus, Endpoint Protection Platform *(EPP)*, Intrusion Detection/Prevention System *(IDS/IPS)*, Extended Detection and Response *(XDR)*, Security Orchestration, Automation, and Response *(SOAR)***, etc. <br>