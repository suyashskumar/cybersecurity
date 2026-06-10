# tcpdump Basics

## Basic Packet Capture

| Command | Explanation |
|----------|-------------|
| `tcpdump -i INTERFACE` | Captures packets on a specific network interface |
| `tcpdump -w FILE` | Writes captured packets to a file |
| `tcpdump -r FILE` | Reads captured packets from a file |
| `tcpdump -c COUNT` | Captures a specific number of packets |
| `tcpdump -n` | Don’t resolve IP addresses |
| `tcpdump -nn` | Don’t resolve IP addresses and don’t resolve protocol numbers |
| `tcpdump -v` | Verbose display; verbosity can be increased with `-vv` and `-vvv` |

## Filtering Expressions

Filtering can be done in 3 ways:
- By Host
- By Port
- By Protocol

Logical operators can come in handy to manipulate filters
- and: captures packets when both conditions are true
- or: captures packets when at least one of the conditions is true
- not: captures packets when the condition is false

| Command | Explanation |
|----------|-------------|
| `tcpdump host IP` or `tcpdump host HOSTNAME` | Filters packets by IP address or hostname |
| `tcpdump src host IP` | Filters packets by a specific source host |
| `tcpdump dst host IP` | Filters packets by a specific destination host |
| `tcpdump port PORT_NUMBER` | Filters packets by port number |
| `tcpdump src port PORT_NUMBER` | Filters packets by the specified source port number |
| `tcpdump dst port PORT_NUMBER` | Filters packets by the specified destination port number |
| `tcpdump PROTOCOL` | Filters packets by protocol; examples include `ip`, `ip6`, and `icmp` |

## Advanced Filtering

- `tcpdump greater LENGTH`: filters packets that have a length greater than or equal to specified length.
- `tcpdump less LENGTH`: filters packets having a length less than or equal to specified length.

- Binary operations:
  - `&` and
    - Returns 1 only if both bits are 1.
  - `|` or
    - Returns 1 unless both bits are 0.
  - `!` not
    - Inverts a bit.

- Header byte filtering syntax
  - `proto[expr:size]`
    - `proto` refers to the protocol, such as `arp`, `ether`, `icmp`, `ip`, `ip6`, `tcp`, or `udp`.
    - `expr` is the byte offset.
    - `size` is the number of bytes and can be 1, 2, or 4. It is optional and defaults to 1.

  - Examples of header byte filtering
    - `ether[0] & 1 != 0`
      - Checks the first byte in the Ethernet header.
      - Used to show packets sent to a multicast address.
    - `ip[0] & 0xf != 5`
      - Checks the first byte in the IP header.
      - Used to catch IP packets with options.

- **TCP flag filtering**: Use `tcp[tcpflags]` to refer to the TCP flags field.
  - Available flags:
    - `tcp-syn` — SYN
    - `tcp-ack` — ACK
    - `tcp-fin` — FIN
    - `tcp-rst` — RST
    - `tcp-push` — PUSH
  - Examples:
    - `tcpdump "tcp[tcpflags] == tcp-syn"`
      - Captures packets with only the SYN flag set.
    - `tcpdump "tcp[tcpflags] & tcp-syn != 0"`
      - Captures packets with at least the SYN flag set.
    - `tcpdump "tcp[tcpflags] & (tcp-syn|tcp-ack) != 0"`
      - Captures packets with at least SYN or ACK set.

## Displaying Packets

| Command | Explanation |
|----------|-------------|
| `tcpdump -q` | Quick and quiet: brief packet information |
| `tcpdump -e` | Include MAC addresses |
| `tcpdump -A` | Print packets as ASCII encoding |
| `tcpdump -xx` | Display packets in hexadecimal format |
| `tcpdump -X` | Show packets in both hexadecimal and ASCII formats |