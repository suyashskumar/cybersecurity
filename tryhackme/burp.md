# Burp Suite: Basics

- Java-based framework, comprehensive solution for conducting web application penetration testing.
- Captures and emables manipulation of all HTTP/HTTPS traffic between a browser and web server.

## Key Features

- **Proxy**: enables interception and modification of requests and responses while interacting with web applications.
- **Repeater**: allows capturing, modification, and resending the same request multiple times. Useful for trial and error through SQL Injections or testing functionalities of an endpoint for vulnerabilities.
- **Intruder**: allows spraying endpoints with requests. Commonly used for brute force attacks or fuzzing endpoints(has rate limitations on the Community version).
- **Decoder**: can decode captured info or encode payloads before sending to target.
- **Comparer**: enables the comparison of two pieces of data at word or byte level.
- **Sequencer**: used to assess randomness of tokens (like session cookie values)

## Understanding Burp GUI

- The dashboard is divided into 4 quadrants:
  1. **Tasks**: allows the definition of background tasks while you use the application. (Example: Live Passive Crawl for logging)
  2. **Event log**: provides info about actions by Burp and connections made through Burp.
  3. **Issue Activity**: displays vulnerabilities identified by automated scanner (privy to Professional version).
  4. **Advisory**: Detailed info about identified vulnerabilities, including references and solutions.

## Navigation Shortcuts

| Shortcut | Tab |
|----------|-----|
| `Ctrl + Shift + D` | Dashboard |
| `Ctrl + Shift + T` | Target tab |
| `Ctrl + Shift + P` | Proxy tab |
| `Ctrl + Shift + I` | Intruder tab |
| `Ctrl + Shift + R` | Repeater tab |

**Note**: You might need to add PortSwigger CA Certificate to a browser's list of trusted certificate authorities manually. To do so, navigate to http://burp/cert. If using Firefox, type *about:preferences* in the URL Bar, View Certificates, and import the *cacert.der* file installed from the Burp URL.