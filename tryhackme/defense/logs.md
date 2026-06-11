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
| 4624 | A user account successfully logged in |
| 4625 | A user account failed to login |
| 4634 | A user account successfully logged off |
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