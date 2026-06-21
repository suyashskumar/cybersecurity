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