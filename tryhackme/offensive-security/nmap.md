# Nmap: Basics

## What can we scan

- **IP address range**: using -, we can specify a range of IP addresses to be scanned. For example, nmap -sn 192.168.0.1-100
- **IP subnet**: using /, we can specify the subnet within which all IP addresses will be scanned. For example, nmap -sn 192.168.0.1/24 would cover 192.168.0.0-255.
- **Hostname**: specify target by hostname

## Host Discovery: Who is Online?

- **For local networks**: systems connected to your Wi-Fi or Ethernet, sends an ARP request
- **For remote networks**: at least one router separates our system from this network, does not send an ARP request

## Flags

| Flags | Description |
| :--- | :--- |
| **Listing** | |
| nmap -sL | lists the targets to scan without scanning them, called a **List Scan** |
| **Host Discovery** | |
| nmap -sn | for both local/remote networks, less noisy and won't tell what services are being run, called a **Ping Scan** |
| **Port Scanning** | |
| nmap -sT |scans TCP ports by completing the three-way handshake then immediately terminating it, called a **Connect Scan** |
| nmap -sS |same thing as -sT but less noisy as it only executes the first step (send SYN packet) leaving the handshake incomplete, called a **SYN scan or Stealth Scan** |
| nmap -sU |scans UDP ports, good for services like DNS, DHCP, NTP, SNMP, VoIP |
| nmap -F |scans as Fast mode, 100 most common ports instead of the default 1000 |
| nmap -p | provide the ports for a range. Example usage: nmap -p- (for all ports from 1-65535), nmap -p-1024 (from ports 1-1024: the well-known ports), nmap -p10-100 (from ports 10-100) |
| nmap -Pn | for hosts that appear down, makes nmap assume all hosts are online |
| nmap -sC | To run default NSE (Nmap Security Engine) scripts. Used alongside -sV for initial recon |
| **Service Detection** | |
| nmap -O | used for OS detection |
| nmap -sV | used for service and version detection |
| nmap -A | for OS, version, and additional detections |
| **Timing** | |
| nmap -T<number> / -T <number> / -T <description> | number can be replaced by any from 0-5. 0 -> paranoid, 1 -> sneaky, 2 -> polite, 3 -> normal, 4 -> aggressive, 5 -> insane, controls the time of scan |
| nmap --min-parallelism <numprobes> / --max-parallelism <numprobes> | usually set by default, but sets number of TCP and UDP parallel port probes active simultaneously for a host group |
| nmap --min-rate <number> / --max-rate <number> | decides rate (number of packets sent by nmap per second) applies to the whole scan, not a single host |
| nmap --host-timeout <time> | amount of time willing to wait before timeout: for slow networks or slow hosts |
| **Real-time Output** | |
| nmap HOSTNAME -v | for verbosity. for more verbosity, -vv/-v2, -vvv/v3 and so on can be used |
| nmap HOSTNAME -d | for debugging, for more debugging outputs, -dd/-d2, -ddd/d3 and so on can be used. -d9 is maximum. |
| **Report** | |
| nmap HOSTNAME -oN FILENAME | normal output to FILENAME.nmap |
| nmap HOSTNAME -oX FILENAME | XML output to FILENAME.xml |
| nmap HOSTNAME -oG FILENAME | grep-able output to FILENAME.gnmap |
| nmap HOSTNAME -oA FILENAME | output in all major formats, like .nmap, .xml, .gnmap for normal, xml and grep-able output respectively. |

**Note**: Use it with sudo if you're not the root user. Otherwise -sS will default to -sT. 