# Wireshark

Wireshark is an open-source, cross-platform network packet analyzer capable of **sniffing** and **investigating live traffic** by inspecting PCAP files (packet captures).

## Understanding Wireshark GUI

| Component | Description |
| :--- | :--- |
| Toolbar | Has menus and shortcuts for sniffing and processing. Processing includes filtering, sorting, summarising, exporting, merging. |
| Display Filter Bar | Query and filtering section. |
| Recent Files | List of recently investigated files. |
| Capture Filter and Interfaces | Capture filters and available sniffing points (network interfaces). The type of software connection (e.g., lo, eth0, ens33) enables network hardware. |
| Status Bar | Tool status, profile, and numeric packet information. |

## Loading PCAP Files

| Component | Description |
| :--- | :--- |
| Packet List Pane | Summary of each packet. Click for further information. |
| Packet Details Panel | Protocol breakdown of selected packet. |
| Packey Bytes Pane | Hex and decoded ASCII representation of the selected packet. |

## Understanding the Packet Details Panel 

The seven layers of the OSI model are shown with respected to the selected packet (from the Packet List Pane). 
- Layer 1: Will show **frame number** and **frame length**, pursuant to the Physical layer.
- Layer 2: Will show **source** and **destination MAC address**, pursuant to the Data Link layer.
- Layer 3: Will show **source** and **destination of the IPv4 addresses**, pursuant to the Network layer.
- Layer 4: Will show details of the protocol used **(TCP/UDP)** along with the **source and destination ports**, pursuant to the Transport layer. Contains **Protocol Errors** section to show which TCP segments need to be reassembled (if any).
- Layer 5: Will show details specific to the protocol used **(HTTP, SMB, FTP, SMTP, etc.)**, pursuant to the Application layer. Contains **Application Data** section that shows application-specific data.

## Packet Navigation

- Go To Packet: Goes to the exact frame number
- Find Packet: Accessible through the **Edit --> Find Packet menu**. Accepts **four input types**: Display filter, Hex, String, Regex (String and Regex are most popular). Also accepts **three fields of search**: Packet List, Packet Details, Packet Bytes. Searches are case-insensitive.
- Mark Packets: Right-click to mark or go to Edit menu. Goes away if application closes (temporary).
- Packet Comments: Right-click to add comments. Stays even if application closes.
- Export Packets: .pcapng extension is used. Packets can be selected before exporting.
- Export Objects (Files): Only supported for **DICOM, HTTP, IMF, SMB, TFTP**.
- Time Display Format: **View --> Time Display Format** to change the order of the packets.
- Expert Info: For colour-based debugging. **Analyze --> Expert Information** or bottom-left of screen. 

| Severity | Colour | Info |
| :--- | :--- | :--- |
| Chat | Blue | Information on usual workflow. |
| Note | Cyan | Notable events. |
| Warn | Yellow | Warnings. |
| Error | Red | Problems. |

| Group | Info |
| :--- | :--- |
| Checksum | Checksum errors. |
| Comment | Packet comment detection. |
| Deprecated | Deprecated protocol usage. |
| Malformed | Malformed packet detection. |

## Packet Filtering

**Capture filters**: For capturing only the packets that are valid for the applied filter.
**Display filters**: For viewing packets valid for the applied filter.

- Apply as Filter: Click on a field, right-click on mouse / **Analyze --> Apply as Filter**. Number of total and displayed packets are on the status bar.
- Conversation Filter: Good for a specific packet and all linked packets (IP addresses and port numbers). Same methods as above.
- Colourise Conversation: Same thing as conversation filter, except you view all the packets and the relevant ones just get coloured, irrelevant ones don't get filtered out.
- Prepare as Filter: Helps create new display filters using right-click.
- Apply as Column: Enable/disable columns on the Packet List pane.
- Follow Stream: Reconstruct stream and view raw traffic. Right-click and select **Follow TCP/UDP/HTTP Stream** or access from Analyze menu. Helps recreate application-level data and understand the event of interest. **Server-side actions are in blue, client-side are in red**, opens in a new window.

**Simple Display Filter Queries**
Use the Apply a Display Filter bar above the Packet List pane.
- Filter by Protocol Name: use keywords like **dhcp, arp, http, smtp, ftp, imap, pop, etc.**
- Filter by Port: use structure **tcp.port == port number** or **udp.port == port number**. Example: "port number" will be replaced by 80.
- Filter by IP: use structure **ip.addr == ip address** where you replace "ip address" with the IP address.