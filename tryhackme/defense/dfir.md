# Digital Forensics Fundamentals

Application of methods and procedures to investigate and solve cyber crimes.

### Phases of Digital Forensics (CEAR)

As defined by the NIST (National Institute of Standards and Technology):

1. **Collection**: Identifying all the devices from which data can be collected
2. **Examination**: Filtering all the unnecessary/irrelevant data out.
3. **Analysis**: Correlating evidence to draw conclusions. Most important phase.
4. **Reporting**: A detailed report consisting an executive summary, investigation methodology, detailed findings, and recommendations.

### Types of Digital Forensics

1. Disk Forensics
2. Network Forensics: Network traffic logs
3. Database Forensics: Intrusion, data modification, data exfiltration
4. Memory Forensics
5. Mobile Forensics: Investigating mobile devices for call and text records, GPS locations, etc.
6. Malware Forensics
7. Email Forensics: Phishing or fraudulent campaigns
8. Cloud Forensics: Investigating cloud infrastructure, can get tricky
9. Computer Forensics: Investigating computers, most common

### Evidence Acquisition General Practices

1. Proper Authorization: from relevant authorities
2. Chain of Custody: proves integrity and reliability of evidence admitted in court through a Chain of Custody document. Includes *description of the evidence **(name, type)**, **name of individual** who collected the evidence, **date and time** of evidence collection, **storage location** of each piece of evidence, **access times and individual record** of who accessed the evidence*. 
3. Use of Write Blockers: Prevent accidental evidence alteration during routine procedures.

### Windows Forensics

Two types of images are prioritized during the data collection phase:
1. Disk image: non-volatile memory stored in HDD, SSD, etc. like documents, web history, etc.
2. Memory image: volatile memory stored in RAM like open files, running processes, etc. Collected first since a shutdown of the system would lead to losing these files.

Tools used:
1. **FTK Imager**: Takes disk images, used for both collection and analysis, friendly GUI.
2. **Autopsy**: Extensive analysis of disk image.
3. **Dumpit**: Takes memory images using CLI.
4. **Volatility**: Analyzing memory images with various plugins.

### CLI-based Investigation Packages

| Tool Name | Description | Installation | Usage |
| :--- | :--- | :--- | :--- |
| pdfinfo | Displays metadata related to a PDF file such as title, author, subject, creator, creation date. | `sudo apt install poppler-utils` | `pdfinfo DOCUMENT.pdf` |
| exiftool | Displays metadata related to an image file (EXIF = Exchangeable Image File Format), containing camera/smartphone model, date and time of capture, photo settings such as focal length, ISO, shutter speed, aperture, and even GPS coordinates. | `sudo apt install libimage-exiftool-perl` | `exiftool IMAGE.jpg` |

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