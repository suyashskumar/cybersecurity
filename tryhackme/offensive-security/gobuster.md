# Gobuster: Basics

Tool used for reconnaissance. Written in Golang. Enumerates web directories, DNS subdomains, vhosts, S3 buckets on AWS, and GCS by brute force.

For the Attack Box on TryHackMe, we needed to change the `/etc/resolv-dnsmasq` file's first line to `nameserver IP-ADDRESS`

## Flags

| Flag | Description |
| :--- | :--- |
| -t | Configures number of **threads** used for the scan. Default value = 10 |
| -w | Configures a **wordlist** for iterating |
| --delay | Defines amount of time to wait before sending requests. Increase the delay to disguise enumeration attempt as regular web traffic. |
| --debug | Troubleshooting during unexpected errors |
| -o | Writes the enumeration results to a file we choose as **output** |

## Commands
- `dir`: uses directory/file enumeration mode
- `dns`: uses DNS subdomain enumeration mode
- `vhost`: uses VHOST enumeration mode with IP address in the URL parameter

## `dir` Command

| Flag | Long Flag | Description |
|------|-----------|-------------|
| `-c` | `--cookies` | This flag configures a cookie to pass along each request, such as a session ID. |
| `-x` | `--extensions` | This flag specifies which file extensions you want to scan for. E.g., `.php`, `.js` |
| `-H` | `--headers` | This flag configures an entire header to pass along with each request. |
| `-k` | `--no-tls-validation` | This flag skips the process that checks the certificate when HTTPS is used. It often happens for CTF events or test rooms like the ones on THM a self-signed certificate is used. This causes an error during the TLS check. |
| `-n` | `--no-status` | You can set this flag when you don't want to see status codes of each response received. This helps keep the output on the screen clear. |
| `-P` | `password` | You can set this flag together with the `--username` flag to execute authenticated requests. This is handy when you have obtained credentials from a user. |
| `-s` | `--status-codes` | With this flag, you can configure which status codes of the received responses you want to display, such as `200`, or a range like `300-400`. |
| `-b` | `--status-codes-blacklist` | This flag allows you to configure which status codes of the received responses you don't want to display. Configuring this flag overrides the `-s` flag. |
| `-U` | `--username` | You can set this flag together with the `--password` flag to execute authenticated requests. This is handy when you have obtained credentials from a user. |
| `-r` | `--followredirect` | This flag configures Gobuster to follow the redirect that it received as a response to the sent request. An HTTP redirect status code (e.g., `301` or `302`) is used to redirect the client to a different URL. |
| `-u` | | For the URL that is supposed to be enumerated |

**Example Usage**: `gobuster dir -u "http://www.example.thm" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -r`

**Note**: `/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt` was used as the wordlist path for our exercise.

## `dns` Command

| Flag | Long Flag | Description |
|------|-----------|-------------|
| `-c` | `--show-cname` | Show CNAME records (cannot be used with the `-i` flag). |
| `-i` | `--show-ips` | Including this flag shows IP addresses that the domain and subdomains resolve to. |
| `-r` | `--resolver` | This flag configures a custom DNS server to use for resolving. |
| `-d` | `--domain` | This flag configures the domain you want to enumerate. **Note**: Do not add "www." before the domain name (because that's a subdomain). The domain name is just that — the domain name. (Example: example.thm instead of www.example.thm) |
| `-w` | `--wordlist` | This flag configures the wordlist to iterate. |

**Example Usage**: `gobuster dns -d example.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt`

**Note**: 
- -d and -w are compulsorily required to be mentioned for the command to work. 
- `/usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt` was the wordlist path for our exercise.
- `dns` mode will do a lookup to the FQDN (Fully Qualified Domain Name) created by combining the configured domain name (-d flag) with an entry of a wordlist. (DIFFERENT FROM VHOST IN THIS MANNER)

## `vhost` Command

| Short Flag | Long Flag | Description |
|------------|-----------|-------------|
| `-u` | `--url` | Specifies the base URL (target domain) for brute-forcing virtual hostnames. |
| | `--append-domain` | Appends the base domain to each word in the wordlist (e.g., `word.example.com`). Will risk false positives if this flag is not added. |
| `-m` | `--method` | Specifies the HTTP method to use for the requests (e.g., `GET`, `POST`). Default is GET. |
| | `--domain` | Appends a domain to each wordlist entry to form a valid hostname (useful if not provided explicitly). Sets the top- and second-level domains in the "Hostname:" part of the request to *example.thm, where example=second-level and .thm=top-level* |
| | `--exclude-length` | Excludes results based on the length of the response body (useful to filter out unwanted responses). Also used to filter out false positives alongside `--append-domain`. Only 200 OK responses are true positives. |
| `-r` | `--follow-redirect` | Follows HTTP redirects (useful for cases where subdomains may redirect). |
| `-w` | `--wordlist` | This flag configures the wordlist to iterate. |

**Example Usage:**: `gobuster vhost -u "http://10.49.158.237" --domain example.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain --exclude-length 250-320`

**Note**:
- `-u` and `-w` are compulsorily required for the command to work.
- `/usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt` is the wordlist used for this exercise.
- `vhost` mode will navigate to the URL created by combining the configured HOSTNAME (-u flag) with an entry of a wordlist. (DIFFERENT FROM DNS IN THIS MANNER)