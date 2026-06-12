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

# Logs Fundamentals

### Use Cases of Logs

- Security Events Monitoring
- Incident Investigation and Forensics
- Troubleshooting
- Performance Monitoring 
- Auditing and Compliance

### Types of Logs

| Log Type | Usage | Example |
|----------|-------|---------|
| System Logs | The system logs can be helpful in troubleshooting running issues in the OS. These logs provide information on various operating system activities. | - System startup and shutdown events<br>- Driver loading events<br>- System error events<br>- Hardware events |
| Security Logs | The security logs help detect and investigate incidents. These logs provide information on the security-related activities in the system. | - Authentication events<br>- Authorization events<br>- Security policy changes events<br>- User account changes events<br>- Abnormal activity events |
| Application Logs | The application logs contain specific events related to the application. Any interactive or non-interactive activity happening inside the application will be logged here. | - User interaction events<br>- Application changes events<br>- Application update events<br>- Application error events |
| Audit Logs | The audit logs provide detailed information on the system changes and user events. These logs are helpful for compliance requirements and can play a vital role in security monitoring as well. | - Data access events<br>- System change events<br>- User activity events<br>- Policy enforcement events |
| Network Logs | Network logs provide information on the network's outgoing and incoming traffic. They play crucial roles in troubleshooting network issues and can also be handy during incident investigations. | - Incoming network traffic events<br>- Outgoing network traffic events<br>- Network connection logs<br>- Network firewall logs |
| Access Logs | The access logs provide detailed information about access to different resources. These resources can be of different types, providing us with information on their access. | - Web server access logs<br>- Database access logs<br>- Application access logs<br>- API access logs |

### Windows Event Log Analysis - Event Viewer

3 crucial types of logs:
- Application: apps running on the OS
- System: the OS itself
- Security: security-related events

4 major fields of the logs:
- Description
- Log Name
- Logged: indicates time
- Event ID: unique identifier

| Event ID | Description |
|----------|-------------|
| 104 | The Windows Event Log was cleared |
| 4624 | A user account successfully logged in |
| 4625 | A user account failed to login |
| 4634 | A user account successfully logged off |
| 4688 | A new process has been created |
| 4720 | A user account was created |
| 4722 | A user account was enabled |
| 4724 | An attempt was made to reset an account's password |
| 4725 | A user account was disabled |
| 4726 | A user account was deleted |

### Web Server Access Logs

Fields:
- IP Address: of the requester
- Timestamp: date and time
- Request
  - HTTP Method (GET/POST/PUT/DELETE)
  - URL
- Status Code: server response
- User-Agent: info about requester's OS and web browser

CLI Usage:
| Command | Description | Example |
| `cat` | Used to read access.log or multiple access.log files | `cat access.log`, `cat access1.log access2.log > combined.log` |
| `grep` | Search strings and patterns inside a log file like IP addresses | `grep "192.168.1.1" access.log` |
| `less` | Helpful for handling multiple log files by creating smaller chunks of data | `less access.log` (`spacebar` to move to next page, `b` to go back, `/` to search, `n` to navigate to next occurrence, `N` to navigate to previous occurrence) |

# SIEM

Types of logs:
- Host-centric logs
  - User accessing a file
  - User attempting to authenticate
  - Process execution activity
  - Process adding/editing/deleting a registry key or value
  - PS Execution
- Network-centric logs
  - SSH connection
  - File being accessed through FTP
  - Web traffic
  - User accessing company resources through VPN
  - Network file sharing activity

| Issues with logs | Features of SIEM that resolve the issues |
| :--- | :--- |
| Numerous log sources (hundreds of events generated per second) | **Dashboards and Reporting** containing details like Alert Highlights, System Notification, Health Alert, List of Failed Login Attempts, Events Ingested Count, Rules Triggered, Top domains Visited |
| No centralization for multiple systems | **Centralized log collection**: Logs pulled through lightweight agents and APIs |
| Limited context | **Correlation of Logs** by finding relationships between individual logs |
| Limited analysis | **Real-time Alerting**: analysts make new decision rules based on their requirements to mature future decisions |
| Format issues | **Normalization of logs** by breaking it down into several fields (Parsing) and converting it into one consistent format. |

Log sources:
- Windows: Event Viewer
- Linux:
  - /var/log/httpd: HTTP Request/Response and error logs.
  - /var/log/cron: Events related to cron jobs
  - /var/log/auth.log and /var/log/secure: Authentication-related logs
  - /var/log/kern: kernel-related events
  - /var/log/apache: for web servers

Log ingestion:
1. Agent/Forwarder: installed on endpoint (example: Splunk)
2. Syslog: protocol
3. Manual upload: Ingest offline data (example: Splunk, ELK - Elasticsearch, Logstash, Kibana)
4. Port forwarding: SIEM listening on a port

# Firewall

## Types of Firewall

| Firewalls | Characteristics |
| :--- | :--- |
| Stateless firewalls | - Layer 3 and 4 of OSI<br>- Basic filtering<br>- No track of previous connections<br>- Efficient for high-speed networks |
| Stateful firewalls | - Layer 3 and 4 of OSI<br>- Recognize traffic by patterns<br>- Complex rules can be applicable<br>- Monitor the network connections |
| Proxy firewalls | - Layer 7 of OSI<br>- Inspect the data inside the packets as well<br>- Provides content filtering options<br>- Provides application control<br>- Decrypts and inspects SSL/TLS data packets |
| Next-generation firewalls | - Layer 3 to 7 of OSI<br>- Provides advanced threat protection<br>- Comes with an intrusion prevention system<br>- Identify anomalies based on heuristic analysis<br>- Decrypts and inspects SSL/TLS data packets |

## Rules in Firewall

| **Basic Components** | **Type of Actions** | **Type of Directions** |
| :--- | :--- | :---|
| Source Address | Allow | Inbound Rules
| Destination Address | Deny | Outbound Rules
| Port | Forward | Forward Rules
| Protocol | | |
| Action | | |
| Direction | | |

## Firewall Tools

1. Windows - Windows Defender Firewall (Advanced Settings)
2. Linux -
  - Netfilter: a framework with core firewall functionalities
    - iptables: widely used utility
    - nftables: advanced version of iptables (enhanced packet filtering and NAT)
    - firewalld: predefined rules, pre-built network zone configs
  - ufw: (uncomplicated firewall) provides an easier interface for beginners

| ufw Commands | Description |
| :--- | :--- |
| `sudo ufw status` | Check the status of the firewall (Enabled.Disabled) |
| `sudo ufw enable/disable` | Enable or disable firewall |
| `sudo ufw default allow/deny outgoing/incoming` | Creating a default policy with type of action and direction |
| `sudo ufw allow/deny <port-number>/<protocol>` | Allow/deny incoming traffic on specific port and protocol. E.g.: `sudo ufw deny 22/tcp` |
| `sudo ufw status numbered` | List all active rules in numbered order | 
| `sudo ufw delete <number>` | Delete a rule using the rule number from the above command |