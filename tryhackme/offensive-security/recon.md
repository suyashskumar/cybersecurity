# Passive Reconnaissance

| Purpose | Command-line Example |
|----------|---------------------|
| Lookup WHOIS record | `whois tryhackme.com` |
| Lookup DNS A records (legacy) | `nslookup -type=A tryhackme.com` |
| Lookup DNS MX records at specific server (legacy) | `nslookup -type=MX tryhackme.com 1.1.1.1` |
| Lookup DNS TXT records (legacy) | `nslookup -type=TXT tryhackme.com` |
| Lookup DNS A records (recommended) | `dig tryhackme.com A` |
| Lookup DNS MX records at specific server (recommended) | `dig @1.1.1.1 tryhackme.com MX` |
| Lookup DNS TXT records (recommended) | `dig tryhackme.com TXT` |
| Passive subdomain discovery (browser-based) | Visit `https://crt.sh` and search `%.tryhackme.com` |

# Active Reconnaissance

| Command | Example |
|----------|----------|
| ping | `ping -c 10 MACHINE_IP` on Linux or macOS |
| ping | `ping -n 10 MACHINE_IP` on Windows |
| ping (IPv6) | `ping -6 MACHINE_IPV6` or `ping6 MACHINE_IPV6` |
| traceroute | `traceroute MACHINE_IP` on Linux or macOS |
| tracert | `tracert MACHINE_IP` on Windows |
| traceroute (IPv6) | `traceroute -6 MACHINE_IPV6` or `traceroute6 MACHINE_IPV6` |
| mtr | `mtr MACHINE_IP` for real-time path monitoring |
| telnet (legacy) | `telnet MACHINE_IP PORT_NUMBER` |
| netcat as client | `nc MACHINE_IP PORT_NUMBER` |
| netcat as server | `nc -lvnp PORT_NUMBER` |
| netcat (IPv6) | `nc -6 MACHINE_IPV6 PORT_NUMBER` |
| curl for HTTP banner | `curl -I http://MACHINE_IP` or `curl -I https://MACHINE_IP` |

# Servers and Protocols

| Protocol | TCP Port | Application(s) | Data Security | Secure Alternative | Secure Port |
|----------|----------|----------------|---------------|-------------------|------------|
| FTP | 21 | File Transfer | Cleartext | FTPS or SFTP | 990 (FTPS), 22 (SFTP) |
| HTTP | 80 | Worldwide Web | Cleartext | HTTPS | 443 |
| IMAP | 143 | Email (MDA) | Cleartext | IMAPS | 993 |
| POP3 | 110 | Email (MDA) | Cleartext | POP3S | 995 |
| SMTP | 25 | Email (MTA) | Cleartext | SMTPS or SMTP with STARTTLS | 465 (SMTPS), 587 (Submission) |
| Telnet | 23 | Remote Access | Cleartext | SSH | 22 |

# Tools

| Tool | Purpose |
|------|---------|
| Wireshark / tcpdump | Network packet capture and analysis |
| Bettercap | MITM attacks, ARP spoofing, and network reconnaissance |
| mitmproxy | Interactive HTTPS proxy for inspecting and modifying traffic |
| testssl.sh | Testing TLS/SSL configuration and vulnerabilities |
| Nmap | Port scanning, service detection, and scripting |
| CrackMapExec / NetExec | Password spraying and lateral movement in Windows environments |
| Hashcat / John the Ripper | Offline password hash cracking |
| Burp Suite | Web application security testing |

# Checklist

When assessing or securing systems, verify the following:

1. All services use TLS 1.2 or TLS 1.3 with strong cipher suites.
2. Cleartext protocols (Telnet, FTP, HTTP) are disabled or restricted to isolated networks.
3. SSH uses key-based authentication with password authentication disabled.
4. Strong password policies are enforced with breached password detection.
5. Account lockout or rate limiting is implemented for all authentication endpoints.
6. Multi-factor authentication is enabled for sensitive systems.
7. Network segmentation limits the impact of sniffing attacks.
8. Certificate validation is properly implemented to prevent MITM attacks.
9. HSTS (HTTP Strict Transport Security) is enabled for web applications to prevent SSL stripping.
10. Logging and monitoring detect authentication anomalies.