# Incident Response Fundamentals

Two types of processes: interactive and non-interactive. <br>
Both these processes generate an event, which is logged. <br>

Alerts that point to something dangerous but are not harmful: False Positives <br>
Alerts that point to something dangerous and are harmful: True Positives (aka Incidents) <br>

Once an "Incident" is established, a severity is assigned: Critical, high, medium, low. <br>

### Types of Incidents

| Incident | Cause | Effect |
| :--- | :--- | :--- |
| Malware Infections | Caused by files that can be text, documents, executables, etc. | Can damage a system, network, or application |
| Security Breaches | Unauthorized personnel gets access to confidential data | Self-explanatory |
| Data Leaks | Human errors or misconfigurations | Confidential information exposed to unauthorized entities for blackmail or reputational damage. |
| Insider Attacks | Hazardous attacks intentionally orchestrated from within the company. | Insider has better access to resources than an outsider. |
| Denial of Service Attacks | Exhaustion of resources available to entertain requests. | Attacker floods a system/network/application with false requests, making it *unavailable* to legitimate users. |

### Incident Response Process (PICERL)

This is the process recommended by SANS:

| Phase | Explanation | Example |
| :--- | :--- | :--- |
| Preparation | Build necessary resources, develop teams, properly plan, and deploy necessary measures. | Awareness training on phishing mails. |
| Identification | Looking for abnormal behaviour using monitoring tools or security solutions. | Security team notices a huge amount of data being sent out from one of the hosts |
| Containment | Minimizing the impact of the attack by isolating victim machine or deactivating compromised accounts. | Security team isolates host from network. |
| Eradication | Removing the threat from the attacked environment. | Deep malware scan executed on attacked system. |
| Recovery | Recovering affected systems from backup or rebuilding them. | Compromised host was re-configured and the exfiltrated data was restored from backup. |
| Lessons Learned | Documentation of gaps in detection and analysis. | Post-incident review meeting. |

The process recommended by NIST has the following differences:
- Identification --> Detection & Analysis
- Containment, Eradication, Recovery is one singular phase
- Lessons Learned --> Post Incident Activity

Incident Response Plan:
- Structured document underlining the approach during any incident.
  - Roles and Responsibilities
  - Methodology
  - Communication plan with stakeholders and law enforcement
  - Escalation path to be followed

Techniques:
- SIEM
- AV (Antivirus)
- EDR
- Playbooks: step-by-step instructions to deal with each kind of Incident
- Runbooks: step-by-step execution of specific steps during different incidents. Resource-dependent.