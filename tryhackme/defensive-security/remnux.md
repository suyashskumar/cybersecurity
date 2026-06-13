# REMNux VM

### oledump - static analysis

`oledump.py` is a python tool to conduct static analysis on potentially malicious documents.
Usage: `oledump.py filename.xlsm/docx`: will print a bunch of data streams. The data stream with `M` implies a `Macro`.
Flags:
- `-s <number>`: to select a data stream (one with Macro preferably)
- `--vbadecompress`: to change the hexdump to readable format

### INetSim: Internet Services Simulation Suite - dynamic analysis

INetSim can simulate a real network.
Steps to configure:

1. `cd /etc/inetsim`
2. `sudo nano inetsim.conf`
3. Under the `dns_default_ip` section, change `# dns_default_ip 0.0.0.0` to `dns_default_ip IP_ADDRESS`
4. Verify with `inetsim.conf | grep "dns_default_ip"`
5. `sudo inetsim` to start the simulator
6. From another system, you can simulate downloading from the network: `sudo wget https://IP_ADDRESS/payload.ps1 --no-check-certificate`
7. Visit the `https://IP_ADDRESS` on browser for its HTML render, which is a blank page declaring a simulated environment.

### Volatility - Preprocessing of memory images

`vol3 -f file.mem` = BASE_COMMAND
`-q` flag is for quiet mode.
`-f` flag is to read from memory capture.

Windows plugins:
- windows.pstree.PsTree: This plugin lists processes in a tree based on their parent process ID.
- windows.pslist.PsList: This plugin is used to list all currently active processes in the machine.
- windows.cmdline.CmdLine: This plugin is used to list process command line arguments.
- windows.filescan.FileScan: This plugin scans for file objects in a particular Windows memory image. The results have more than 1,400 lines.
- windows.dlllist.DllList: This plugin lists the loaded modules in a particular Windows memory image. Due to a text limitation, this one won't have a View Results icon.
- windows.malfind.Malfind:  This plugin is used to lists process memory ranges that potentially contain injected code. There won't be any View Results icon for this one due to text limitation.
- windows.psscan.PsScan: This plugin is used to scan for processes present in a particular Windows memory image.

Usage: `BASE_COMMAND PLUGIN`
Example: `vol3 -f file.mem windows.pstree.PsTree`

Saving it to a file: `for plugin in windows.malfind.Malfind windows.psscan.PsScan windows.pstree.PsTree windows.pslist.PsList windows.cmdline.CmdLine windows.filescan.FileScan windows.dlllist.DllList; do vol3 -q -f wcry.mem $plugin > wcry.$plugin.txt; done`<br>
Simplified version: `for plugin in <plugins>; do vol3 -q -f file.mem $plugin > file.$plugin.txt; done` where "plugins" is from the list above

### Preprocessing with Strings - Linux

- `strings file.mem > file.strings.ascii.txt`: To extract ASCII text
- `strings -e l  file.mem > file.strings.unicode_little_endian.txt`: to extract 16-bit little-endian text
- `strings -e b  file.mem > file.strings.unicode_big_endian.txt`: to extract 16-bit big-endian text

Note: Filename.txt can have any file name. The flags are the important part.<br>

- No flag: ASCII
- `-e l`: little-endian
- `-e b`: big-endian