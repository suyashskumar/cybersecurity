# OWASP Top 10

IAAA: A simple way to think about how users and their actions are verified on applications.

**Identity**: The unique account that represents a person or service
**Authentication**: Proving that identity
**Authorisation**: What that identity is allowed to do
**Accountability**: Reordering or alerting on who did what, when, where

# A01: Broken Access Control

**Broken Access Control happens when the server doesn’t properly enforce who can access what on every request**. A common occurence of this is IDOR (Insecure Direct Object Reference): if changing an ID (like ?id=7 → ?id=6) lets you see or edit someone else’s data, access control is broken.

In practice this shows up as **horizontal privilege escalation** (same role, other user’s stuff) or **vertical privilege escalation** (jumping to admin-only actions) because the application trusts the client too much.

Solution: Enforce server-side checks on every request

# A04: Cryptographic Failures

Cryptographic failures happen when sensitive data isn't adequately protected due to lack of encryption, faulty implementation, or insufficient security measures. 

Preventing cryptographic failures starts with choosing strong, modern algorithms and implementing them properly. Sensitive information such as passwords should be hashed using robust, slow hashing functions like bcrypt, scrypt, or Argon2. When encrypting data, avoid creating your own algorithms; instead, rely on trusted, industry-standard libraries.

Never embed access credentials (i.e., to a third-party service) in source code, configuration files, or repositories. Instead, use secure key management systems or environments specifically designed for storing secrets.

# A05: Injection

Injection occurs when an application takes user input and mishandles it. Instead of processing the input securely, the application passes it directly into a system that can execute commands or queries, such as a database, a shell, a templating engine or API.

Preventing injection starts by ensuring that user input is always treated as untrusted. Rather than parsing directly, instead, take elements of the input for querying. 
- For SQL queries, this means using prepared statements and parameterised queries instead of building queries through string concatenation. 
- For OS commands, avoid functions that pass input directly to the system shell, and instead rely on safe APIs and processes that don’t invoke the shell at all.

Input validation and sanitisation play a crucial role in preventing these types of attack. Escape dangerous characters, enforce strict data types and filter before the application even processes the input.

Example command using Jinja2 to expose Flask globals: `{{request.application.__globals__.__builtins__.open('flag.txt').read() }}`

# A07: Authentication Failures

Authentication Failures happen when an **application can’t reliably verify or bind a user’s identity**. Common issues include:
- username enumeration
- weak/guessable passwords (no lockout/rate limits)
- logic flaws in the login/registration flow
- insecure session or cookie handling

If any of these are present, an attacker can often log in as someone else or bind a session to the wrong account.

Solution: Enforce unique indexes on the canonical form, rate-limit/lock out brute force, and rotate sessions on password/privilege changes.

# A08: Software or Data Integrity Failures

Software or Data Integrity Failures occur when an application relies on code, updates, or data it assumes are safe, without verifying their authenticity, integrity , or origin. This includes trusting software updates without verification, loading scripts or configuration files from untrusted sources, failing to validate data that impacts application logic, or accepting data such as binaries, templates, or JSON files without confirming whether it has been altered.

Preventing these failures begins with establishing trust boundaries. Applications should never assume that code, updates, or key pieces of data are legitimate and automatically trusted; their integrity must be verified. This involves using methods such as cryptographic checks (like checksums) for update packages and ensuring that only trusted sources can modify critical artefacts.

Additionally, for applications, integrity and trust boundaries should also be within build processes such as CI/CD.

Example usage:
```python
import pickle
import base64

class Deserialize:
    def __reduce__(self):
        # Return a tuple: (callable, args)
        # This will execute: open('flag.txt').read()
        return (eval, ("open('flag.txt').read()",))

# Generate and encode the payload
payload = pickle.dumps(Deserialize())
encoded = base64.b64encode(payload).decode()
print(encoded)

# Copy the output and paste it into the form
```

- Create a Python script to generate a malicious pickle payload that reads `flag.txt`
- The payload should use pickle's `__reduce__` to execute `open('flag.txt').read()`
- Base64 encode the pickle payload
- Submit the base64-encoded payload to the form
- The deserialized output will contain the flag


# A09: Login and Alerting Failures

When applications don’t record or alert on security-relevant events, defenders can’t detect or investigate attacks. In practice, failures look like **missing authentication events, vague error logs, no alerting on brute-force or privilege changes, short retention, or logs stored where attackers can tamper with them.**

Solution: Log the full auth lifecycle (fail/success, password/2FA/role changes, admin actions), centralise logs off-host with retention, and alert on anomalies (e.g., brute-force bursts, privilege elevation).

# AS02: Security Misconfigurations

Security misconfigurations happen when systems, servers, or applications are deployed with unsafe defaults, incomplete settings, or exposed services. These are not code bugs but **mistakes in how the environment, software, or network is set up**. They create easy entry points for attackers.

Common patterns:
- Default credentials or weak passwords left unchanged 
- Unnecessary services or endpoints exposed to the internet 
- Misconfigured cloud storage or permissions (, Azure Blob, GCP buckets) 
- Unrestricted access or missing authentication/authorisation 
- Verbose error messages exposing stack traces or system details 
- Outdated software, frameworks, or containers with known vulnerabilities 
- Exposed AI/ML endpoints without proper access controls 

How to prevent it:
- Harden default configurations and remove unused features or services
- Enforce strong authentication and least privilege across all systems
- Limit network exposure and segment sensitive resources
- Keep software, frameworks, and containers up to date with patches
- Hide stack traces and system information from error messages
- Audit cloud configurations and permissions regularly
- Secure AI endpoints and automation services with proper access controls and monitoring
- Integrate configuration reviews and automated security checks into your deployment pipeline

# AS03: Software Supply Chain Failures

Software supply chain failures happen when applications rely on components, libraries, services, or models that are compromised, outdated, or improperly verified.

Common patterns:
- Using unverified or unmaintained libraries and dependencies
- Automatically installing updates without verification
- Over-reliance on third-party AI models without monitoring or auditing
- Insecure build pipelines or CI/CD processes that allow tampering
- Poor license or provenance tracking for components
- Lack of monitoring for vulnerabilities in dependencies after deployment

Prevention:
- Verify all third-party components, libraries, and AI models before use
- Monitor and patch dependencies regularly
- Sign, verify, and audit software updates and packages
- Lock down CI/CD pipelines and build processes to prevent tampering
- Track provenance and licensing for all dependencies
- Implement runtime monitoring for unusual behaviour from dependencies or AI components
- Integrate supply chain threat modelling into the SDLC, including testing, deployment, and update workflows

# AS04: Cryptographic Failures

Cryptographic failures happen when encryption is used incorrectly or not at all. This includes weak algorithms, hard-coded keys, poor key handling, or unencrypted sensitive data. These flaws let attackers access information that should be private.

Common patterns:

- Using deprecated or weak algorithms like MD5, SHA-1, or ECB mode
- Hard-coded secrets in code or configuration
- Poor key rotation or management practices
- Lack of encryption for sensitive data at rest or in transit
- Self-signed or invalid TLS certificates
- Using AI/ML systems without proper secret handling for model parameters or sensitive inputs

Prevention:

- Use strong, modern algorithms such as AES-GCM, ChaCha20-Poly1305, or enforce TLS 1.3 with valid certificates
- Use secure key management services like Azure Key Vault, AWS KMS, or HashiCorp Vault
- Rotate secrets and keys regularly, following defined crypto periods
- Document and enforce policies and standard operating procedures for key lifecycle management
- Maintain a complete inventory of certificates, keys, and their owners
- Ensure AI models and automation agents never expose unencrypted secrets or sensitive data
- The web application in this room contains a weakness of this type for you to explore.

# AS06: Insecure Designs

Insecure design happens when flawed logic or architecture is built into a system from the start. These flaws stem from skipped threat modelling, no design requirements or reviews, or accidental errors.

Common patterns:
- Weak business logic controls, like recovery or approval flows
- Flawed assumptions about user or model behaviour
- AI components with unchecked authority or access
- Missing guardrails for LLMs and automation agents
- Test or debug bypasses left in production
- No consistent abuse-case review or AI threat modelling

Prevention:
- Treat every model as untrusted until proven otherwise.
- Validate and filter all model inputs and outputs to ensure accuracy and integrity .
- Separate system prompts from user content.
- Keep sensitive data out of prompts unless absolutely needed and protect it with strict controls.
- Require human review for high-risk AI actions.
- Log model provenance, monitor behaviour, and apply differential privacy for sensitive data.
- Include AI-specific threat modelling for prompt attacks, inference risks, agent misuse, and supply chain compromise throughout the design process.
- Build threat modelling into every stage of development, not just at the start.
- Define clear security requirements for each feature before implementation.
- Apply the principle of least privilege across users, APIs, and services.
- Ensure proper authentication, authorisation, and session management across the system.
- Keep dependencies, third-party components, and supply chain sources verified and up to date.
- Continuously monitor and test the system for logic flaws, abuse paths, and emergent risks as new features or AI components are added. 