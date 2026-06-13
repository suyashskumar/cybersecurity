# CAPA - Common Analysis Platform for Artifacts

Flags:
- -h / --help
- -v / --verbose
- -vv / --vverbose
- -j (direct results to JSON)

## Dissecting CAPA Results

### Part 1: General Info, MITRE ATT&CK, MAEC

**General Information**:

- cryptographic algorithm
- analysis field: tells us how CAPA performed the analysis
- os field: reveals the os context 
- arch field: tells us architecture
- path field: where the file was located

**MITRE ATT&CK (Adversarial Tactics, Techniques, and Common Knowledge) Framework**:

| Format | Sample | Explanation |
|----------|----------|----------|
| ATT&CK Tactic::ATT&CK Technique::Technique Identifier | Defense Evasion::Obfuscated Files or Information::T1027 | **DEFENSE EVASION** = ATT&CK Tactic<br>**Obfuscated Files or Information** = ATT&CK Technique<br>**T1027** = Technique Identifier |
| ATT&CK Tactic::ATT&CK Technique::ATT&CK Sub-Technique::Technique Identifier[.]Sub-technique Identifier | Defense Evasion::Obfuscated Files or Information::Indicator Removal from Tools T1027.005 | **DEFENSE EVASION** = ATT&CK Tactic<br>**Obfuscated Files or Information** = ATT&CK Technique<br>**Indicator Removal from Tools** = ATT&CK Sub-Technique<br>**T1027** = Technique Identifier<br>**005** = Sub-Technique Identifier |

**MAEC (Malware Attribute Enumeration and Characterization)**:

| MAEC Value | Description | Common Behaviours |
|------------|-------------|-------------------|
| Launcher | Exhibits behaviours that trigger specific actions similar to malware behaviour. | - Dropping additional payloads<br>- Activating persistence mechanisms<br>- Connecting to command-and-control (C2) servers<br>- Executing specific functions |
| Downloader | Exhibits behaviours wherein it downloads and executes other files, usually seen on more complex malware. | - Fetching additional payloads or resources from the internet<br>- Pulling in updates<br>- Executing secondary stages<br>- Retrieving configuration files |

### Part 2: MBC (Malware Behaviour Catalogue)

**Format**:

| Format | Sample | Explanation |
|----------|----------|----------|
| OBJECTIVE::Behavior::Method[Identifier] | ANTI-STATIC ANALYSIS::Executable Code Obfuscation::Argument Obfuscation [B0032.020] | **Anti-static Analysis** = Objective<br>**Executable Code Obfuscation** = Behavior<br>**Argument Obfuscation** = Method<br>**B0032.020** = Identifier |
| OBJECTIVE::Behavior::[Identifier] | COMMUNICATION::HTTP Communication:: [C0002] | **COMMUNICATION** = Objective<br>**HTTP Communication** = Behavior<br>**C0002** = Identifier |

Dissecting each component of the MBC:

**Objective**: based on ATT&CK tactics in the context of malware behaviour

| Objective | Explanation |
|------------|-------------|
| Anti-Behavioral Analysis | Malware attempts to avoid detection by hindering behavioural analysis using tools like sandboxes or debuggers. |
| Anti-Static Analysis | Malware attempts to obstruct or add complexity to static analysis, making it more challenging for security professionals to identify and understand its malicious behaviours and intentions. |
| Collection | Malware focuses on identifying and gathering information from the targeted machine or network. |
| Command and Control | Malware typically establishes communication with compromised systems through various methods such as command and control servers, peer-to-peer networks, or other means. This communication allows the malware to control the compromised systems, enabling the attackers to execute commands, exfiltrate data, or carry out other malicious activities. |
| Credential Access | The primary aim of malware is to steal account credentials, such as usernames and passwords. |
| Defense Evasion | The malware aims to bypass and circumvent the various detection and security mechanisms present within the system to avoid being detected or mitigated. |
| Discovery | Malware seeks to collect detailed information about the configuration and setup of the system or network environment, including hardware, software, and network infrastructure. |
| Execution | Malware is designed to execute unauthorized commands or code on a targeted computer system without the user's consent. This can include a wide range of harmful activities, such as stealing personal information, damaging files, or gaining unauthorized access to the system. |
| Exfiltration | Malware is designed to infiltrate computer systems or networks to steal and extract sensitive data. This can include personal information, financial details, and any other valuable data stored on the targeted system or network. |
| Impact | Malware aims to manipulate, disrupt, or damage computer systems and data. It can enter computers through infected emails, compromised websites, and other deceptive means, leading to financial loss, privacy breaches, and system instability. |
| Lateral Movement | Malware seeks to spread through the network, either actively through machine access or passively, such as via malicious emails. |
| Persistence | Malware is intentionally developed with the capability to remain undetected and operational on a computer system for an extended period. |
| Privilege Escalation | Malware seeks to infiltrate a computer system or network to gain elevated permissions or control. Once inside the target environment, malware can seek to escalate its privileges, access sensitive information, or take control of system resources for malicious purposes. |

**Micro-objective**: associated with micro-behaviors, which refer to an action or actions exhibited by potentially malicious software that isn't necessarily malicious and may serve various objectives

| Micro-Objective | Description |
|----------------|-------------|
| PROCESS | Exhibiting behaviours related to processes such as, but not limited to, Creating Process, Setting Thread Context, Terminating Process, and Checking Mutex. |
| MEMORY | Exhibiting behaviours such as, but not limited to, Allocating Memory, Changing Memory Protection, and Freeing Memory. |
| COMMUNICATION | Exhibiting behaviours such as (not limited to) DNS, FTP, HTTP, ICMP, and SMTP network traffic. |
| DATA | Exhibiting behaviours such as, but not limited to, Checking Strings, Compressing Data, Decoding Data, and Encoding Data. |

**MBC Behaviors**: 

| Objective | Behavior | Identifier | Explanation |
|-----------|----------|------------|-------------|
| ANTI-BEHAVIORAL ANALYSIS | Lab Machine Detection | B0009 | The malware checks to see if it is running in a virtual environment. During its system reconnaissance, the malware examines various user and system artifacts. |
| ANTI-STATIC ANALYSIS | Executable Code Obfuscation | B0032 | Executable code has been intentionally obscured to prevent static code analysis. This is a specific behavior related to the executable code of a malware sample, including its data and text sections. |
| EXECUTION | Command and Scripting Interpreter | E1059 | Malware can exploit command and script interpreters to run malicious commands, scripts, or binaries. It targets built-in interpreters like cmd.exe or PowerShell on Windows or Bash on Unix-like systems. Attackers may also use other scripting languages like Python, Perl, or JavaScript. |
| DISCOVERY | File and Directory Discovery | E1083 | Malware has the capability to search for specific files in particular locations by enumerating files and directories. |
| ANTI-STATIC ANALYSIS, DEFENSE EVASION | Obfuscated Files or Information | E1027 | Malware can obfuscate files or information by encoding, encrypting, or otherwise making them hard to analyze. It can also encode or encrypt malware samples itself. |

**MBC Micro-behaviours**: The term "low-level behaviors" in malware analysis refers to actions exhibited by malware that aren't necessarily malicious and may serve various objectives.

| Micro-Objective | Micro-Behavior | Identifier | Explanation |
|----------------|----------------|------------|-------------|
| MEMORY | Allocate Memory | C0007 | Malware frequently utilizes memory allocation as part of its strategy to unpack itself and execute its malicious activities. |
| PROCESS | Create Process | C0017 | Malware creates a process via WMI or shellcode. It can also create a suspended process. |
| COMMUNICATION | HTTP Communication | C0002 | Malware is capable of initiating HTTP communications. |
| DATA | Check String | C0019 | Malware can inspect a string to identify specific characteristics, such as ASCII content, credit card numbers, and string length. |
| DATA | Encode Data | C0026 | Malware has the capability to encode data using Base64 and XOR. |
| FILE SYSTEM | Create Directory | C0046 | Malware can create a directory. |
| FILE SYSTEM | Delete File | C0047 | Malware has the capability to delete a file. |
| FILE SYSTEM | Read File | C0051 | Malware can read a file. |
| FILE SYSTEM | Write File | C0052 | Malware has the capability to write to a file. |

**Methods**:

| Behavior | Method / Sub-Technique | Identifier | Explanation |
|----------|------------------------|------------|-------------|
| Executable Code Obfuscation | Argument Obfuscation | B0032.020 | Simple number or string arguments to API calls are calculated at runtime, making analysis more difficult. |
| Executable Code Obfuscation | Stack Strings | B0032.017 | Build and decrypt strings on the stack at each use, then discard them to avoid obvious references. |
| HTTP Communication | Read Header | C0002.014 | HTTP read header. |
| Encode Data | Base64 | C0026.001 | Malware may encode data using Base64. |
| Encode Data | XOR | C0026.002 | Malware may use XOR to encode data. |
| Obfuscated Files or Information | Encoding-Standard Algorithm | E1027.m02 | Encoding malware samples, files, or other information uses a standard algorithm (e.g., Base64). |

### Part 3: Namespaces

**Format**:

| Format | Sample | Explanation |
|----------|----------|----------|
| Capability(Rule Name)::TLN(Top-Level Namespace)/Namespace | reference anti-VM strings::Anti-Analysis/anti-vm/vm-detection | **Reference anti-VM strings** = Capability (Rule Name)<br>**Anti-Analysis** = TLN (Top-Level Namespace)<br>**anti-vm/vm-detection** = Namespace |

**Top-Level Namespaces (TLN)**:

| Top-Level Namespace (TLN) | Explanation |
|--------------------------|-------------|
| anti-analysis | Contains a set of rules specifically designed to detect behaviours exhibited by malware to evade analysis. These behaviours include obfuscation, packing, and anti-debugging techniques. |
| collection | Contains a set of data-related rules that malware may enumerate and collect for exfiltration or other purposes. Think of it as the "data-gathering" aspect of malware behaviour. |
| communication | Contains a set of rules that pertain to different communication behaviours demonstrated by malware. This encompasses how malware interacts with networks, including data transmission and reception, command and control communications, and other network-related behaviours. |
| compiler | Contains a set of rules and configurations for recognizing specific build environments or compilers employed in generating executables. These namespaces essentially serve as the unique "signature" that identifies the compilation process of a program. |
| data-manipulation | Contains a set of rules that govern the behaviours involved in altering data within executable files. This aspect can be considered the "data transformation" component of malware behaviour, encompassing actions such as String Encryption and Data Encoding. |
| executable | Contains a set of rules pertaining to the attributes in executable files. These attributes include PE sections or debug information associated with the executable. |
| host-interaction | Contains a set of rules related to behaviours involving interactions with the host system. This encompasses how malware interacts with its environment. Specifically, the rules in this namespace may capture behaviours related to reading, writing, or modifying files on disk, including creating, deleting, or modifying files and directories. |
| impact | Contains a set of rules related to the potential consequences or effects of a program's behavior. Think of it as the aspect that focuses on the possible harm that malware can cause. It may include behaviors related to establishing remote access, data exfiltration, destruction, or modification. |
| internal | Rules contained within the system are not intended for direct use by analysts or for reporting. Instead, these rules are meant for internal purposes within the CAPA tool, serving as the behind-the-scenes aspect of rule development and execution. |
| lib | Building blocks used to create other rules. |
| linking | Contains rules to identify behaviors involving linking or dynamically loading external code or libraries during program execution. This is its primary function and is crucial for the program's security. Understanding linking behavior is essential for several reasons. Malware often depends on external libraries or components (such as OpenSSL, Zlib, or other third-party libraries) to carry out specific tasks. Detecting these dependencies helps analysts understand the malware's capabilities. External libraries also introduce an additional attack surface. If a vulnerability exists in a linked library, it can be exploited by the malware or defenders during analysis. |
| load-code | Contains a set of rules and regulations related to the behaviors associated with dynamically loading or executing code during program execution. This concept can be equated to the "runtime code injection" aspect of malware behavior, which involves unauthorized code introduction during a program's execution. |
| malware-family | Contains a set of rules related to behaviors that are linked to particular malware families or groups. It serves as a way to identify the distinct characteristics or "signatures" associated with known malware families, allowing for more accurate detection and classification of potential threats. |
| nursery | A staging ground that contains rules for those that are not quite polished. |
| persistence | Contains rules related to behaviors associated with maintaining access or persistence within a compromised system. This namespace is essentially focused on understanding how malware can establish and maintain a presence within a compromised environment, allowing it to persist and carry out malicious activities over an extended period. |
| runtime | Contains a set of rules that seek to identify the language or platform on which the program runs. |
| targeting | Contains a set of rules related to behaviors exhibited by programs that interact with ATMs. |

**Namespaces**: Come under TLN, are grouped set of rules written in YML.

EXAMPLE:

| Top-Level Namespace (TLN) | Namespace | Rule YAML File | Explanation |
|--------------------------|-----------|----------------|-------------|
| Anti-Analysis | anti-vm/vm-detection | reference-anti-vm-strings-targeting-virtualbox.yml | The **anti-vm/vm-detection** namespace contains rules to detect lab machine (VM) environments. These rules focus on identifying specific strings or patterns commonly used by malware to detect VMs while running. Using these rules, CAPA can identify if malware searches for VMware-specific registry keys, the presence of VMware tools, or other VM-related elements. |
| Anti-Analysis | anti-vm/vm-detection | reference-anti-vm-strings-targeting-virtualpc.yml | The **anti-vm/vm-detection** namespace contains rules to detect lab machine (VM) environments. These rules focus on identifying specific strings or patterns commonly used by malware to detect VMs while running. Using these rules, CAPA can identify if malware searches for VMware-specific registry keys, the presence of VMware tools, or other VM-related elements. |
| Anti-Analysis | obfuscation | obfuscated-with-dotfuscator.yml | Malware often uses obfuscation techniques to make analysis more difficult. These include methods such as String Encryption, Code Obfuscation, Packing, and Anti-Debugging Tricks. The **obfuscation** namespace addresses these techniques, which conceal or obscure the true purpose of the code. |
| Anti-Analysis | obfuscation | obfuscated-with-smartassembly.yml | Malware often uses obfuscation techniques to make analysis more difficult. These include methods such as String Encryption, Code Obfuscation, Packing, and Anti-Debugging Tricks. The **obfuscation** namespace addresses these techniques, which conceal or obscure the true purpose of the code. |

### Part 4: Capability

Capability is just the name of the rule.

| Capability | Top-Level Namespace (TLN) | Namespace | Rule YAML File |
|------------|--------------------------|-----------|----------------|
| reference anti-VM strings | Anti-Analysis | anti-vm/vm-detection | reference-anti-vm-strings.yml |
| reference anti-VM strings targeting VMware | Anti-Analysis | anti-vm/vm-detection | reference-anti-vm-strings-targeting-vmware.yml |
| reference anti-VM strings targeting VirtualBox | Anti-Analysis | anti-vm/vm-detection | reference-anti-vm-strings-targeting-virtualbox.yml |
| reference HTTP User-Agent string | Communication | http/client | reference-http-user-agent-string.yml |
| check HTTP status code | Communication | http | check-http-status-code.yml |
| reference Base64 string | Data Manipulation | encoding/base64 | reference-base64-string.yml |
| encode data using XOR | Data Manipulation | encoding/XOR | encode-data-using-xor.yml |
| contain a thread local storage (.tls) section | Executable | pe/section/tls | contain-a-thread-local-storage-tls-section.yml |
| get common file path | Host-Interaction | file-system | get-common-file-path.yml |
| create directory | Host-Interaction | file-system/create | create-directory.yml |
| delete file | Host-Interaction | file-system/delete | delete-file.yml |
| read file on Windows | Host-Interaction | file-system/read | read-file-on-windows.yml |
| write file on Windows | Host-Interaction | file-system/write | write-file-on-windows.yml |
| get thread local storage value | Host-Interaction | process | get-thread-local-storage-value.yml |
| allocate or change RWX memory | Host-Interaction | process/inject | allocate-or-change-rwx-memory.yml |
| create process on Windows | Host-Interaction | process/create | create-process-on-windows.yml |
| reference cryptocurrency strings | Impact | impact/cryptocurrency | reference-cryptocurrency-strings.yml |
| link function at runtime on Windows | Linking | runtime-linking | link-function-at-runtime-on-windows.yml |
| parse PE header | load-code | load-code/pe | parse-pe-header.yml |
| resolve function by parsing PE exports | load-code | load-code/pe | resolve-function-by-parsing-pe-exports.yml |
| run PowerShell expression | load-code | load-code/PowerShell | run-powershell-expression.yml |
| schedule task via at | persistence | scheduled-tasks | schedule-task-via-at.yml |
| schedule task via schtasks | persistence | scheduled-tasks | schedule-task-via-schtasks.yml |