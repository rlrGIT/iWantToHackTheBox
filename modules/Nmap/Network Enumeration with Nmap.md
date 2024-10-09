"Enumeration is the most critical part of all."
	*-Someone who knows more than me*
### Enumeration
The goal of enumeration is to improve our understanding of the technologies, protocols in order to build upon previously acquired information. This helps us enumerate attack vectors, the source of exploits. The two important ways we narrow down information are:

1) Functions and or resources that allow us to interact with the target and/or provide additional information.
2) Information that provides us with even more important information to access our target.

Manual enumeration is critical - automation often has layers of complexity abstracted away from users. This means, without a deep understanding of the source code you are using, you don't actually know if the code being run is actually doing what you think it is. For example, if nmap does not receive a response from a port or service within a certain time, it can mark a port as closed, filtered, or unknown. We don't know this unless we have a deeper understanding of how nmap runs automated behavior.

### Nmap receives different flags in responses from targets, which help us determine firewall details
When running a default scan, nmap will use the `-sS` option, which runs a **TCP-SYN** scan (with the synchronize flag). This method can scan thousands of ports per second, and works by sending the **SYN** flag. This process is the first step of the three-way handshake that establishes a complete connection to a port. Open ports respond with the **SYN-ACK** flag, which *acknowledges the clients attempt to connect*. 

A quick overview of responses is:
- returned **SYN-ACK** - interpreted as the port being open (S)
- returned **RST** (reset) - interpreted as closed - host signaling to abort connection (RA - RST and ACK flags)
- returned NONE - interpreted as filtered, a firewall might be dropping or ignoring packets

TCP headers also contain other information that can help us narrow down OS fingerprints: `ttl=56 id=57322 iplen=44 seq=1699105818 win=1024 mss 1460` 

### Hosts are efficiently discovered with ICMP echo requests, but by default nmap will start with ARP-ping
**Pro tip:** store every scan, and read more about [Host Discovery](https://nmap.org/book/host-discovery-strategies.html) You can use options like `-oN, -oX, -oG` for outputting to a file that is normal, xml, or grep-able, respectively.

The most effective way of doing this is to use an **ICMP echo request**. When these requests are sent, nmap expects an ICMP reply if the host is alive. This method of scanning only works when firewalls or hosts allow it, but virtually all devices will respond to ping if it is allowed. A percentage of successful probes is stored next to a common list of options on the nmap website (see host discovery link).

The options that I have found that make for relatively fast, but also not overly intrusive scans *host scanning*:
- `-iL <filename>` can be used to pass a series of IP's to nmap separated with newlines 
- `-sn` disable port scanning
- `-PE` explicit call to use **ICMP Echo Requests**; `-Pn` disables this. Interestingly, nmap will default to sending an **?ARP ping** and expect an **?ARP reply** by default.
- `--disable-arp-ping`
- `--reason` explicitly says why nmap has marked a target as alive

Debugging or verbose commands can help us determine what method nmap is using, and we can see individual packet responses to verify any suspicions we have:
- `--packet-trace` enables us to examine the type of packet sent and received, as well as some other information like IP and perhaps a MAC address

### Interpreting Scanned Hosts and Ports

We want to look for:
- Open ports and services
- Service versions
- Service information
- Operating system info

What port states mean, kinda. It is important to examine packet flags to make sure we are getting the full picture.

| State            | Description                                                                                                                                                                             |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| open             | Connection has been ESTABLISHED through either TCP connections, UDP datagrams, or ?SCTP associations                                                                                    |
| closed           | When a port is shown as closed, we receive an RST Flag (see above)                                                                                                                      |
| filtered         | Probably firewall. But we don't know for sure because this can either mean no response from the target or some error code from the target. You should investigate packets more closely. |
| unfiltered       | Specific to TCP-ACK scan, means the port is accessible, but unknown if open or closed                                                                                                   |
| open\|filtered   | If we do not get a response from a specific port; probably firewall or packet filter.                                                                                                   |
| closed\|filtered | Specific to IP ID idle scan, indicates impossible to dtermine if the scanned port is closed or filtered by a firewall :(                                                                |
### Syn scans are fast, but less stealthy cause they use raw packets and do not complete connections

When running as root, nmap will default to `-sS` (syn-scan) on the top 1000 ports (`--top-ports=1000`). This is because root permissions are required to create raw TCP packets at the socket level. A pure syn-scan may disable things like arp-ping, ?DNS, and ICMP echo requests (presumably, we already established a host to scan). We can also look for specific response flags with `--packet-trace` to confirm our suspicions.

`sudo nmap <ip> -n -Pn --disable-arp-ping --packet-trace`

Such a scan is focused purely on scanning ports; I believe that scans, like functions in code, should (attempt to) do one thing as much as possible.

### Connect scans are slower, but the most stealthy because they complete connections and handshakes

If root permission is not an option, the `-sT` (TCP-scan) option is used by default. This method, unlike the `-sS`, fully utilizes a 3-way TCP handshake. While this is slower because we have to wait for the target to respond, it is much less likely to be detected by **Intrusion Detection Systems (IDS)** and **Intrusion Prevention Systems (IPS)**, which may pick up on consistently unsent packets or unfinished connections on a target host. These kinds of events can sometimes disturb the services running on these ports, which can make a scan "louder".

### Connect scans can also be used to gather information about firewall rules
TCP scans are also useful for targeting hosts with personal firewalls that drops incoming packets, but allows outgoing packets. In this particular case, a Connect scan `(-sT)` can bypass the firewall and determine the state of the ports. (How?)

In most cases, firewalls have certain rules to handle specific kinds of connections. Packets can either be dropped or rejected. When a packet is dropped, nmap does not receive a response, and the default retry rate (`--max-retries=1`) will attempt to double check that the missing packet was not mishandled.

### Time and error messages can be used to determine whether a firewall drops or rejects packets  
When we do no see any `RCVD` packets with `--packet-trace`, we know that packets have been dropped. But we can also use time. When a firewall drops packets, the duration of the scan is typically much longer since, nmap will automatically try to determine whether packets were mishandled. But when packets are rejected, we do not need to perform this checking. 

If we receive (RCVD) a message with type or error code, we can suspect (somewhat strongly) that the desired port is behind a firewall rejecting packets. 

### Port Specification
- use `-p-` to specify all ports
- use `-p<int>-<int>` to specify a range: `-p69-420` - this is probably useful for scripting
- `--top-ports=<int>` range of top ports
- `-F` "fast"; top 100 ports only

### UDP takes longer to scan, because as part of the protocol, responses are not expected and there is no acknowledgment. But sometimes admins forget about securing UDP ports

A UDP scan with `-sU` must have a longer retry timeout. Because nmap sends empty datagrams to UDP ports, a response is usually not even sent. This makes it hard to determine if our packets have arrived or not. If we receive ICMP error 3, (port unreachable), however, we know that the port is closed. However, nmap will interpret all other responses that are not error code 3 as `open|filtered`.

### Service enumeration takes a long time, but there is information that we can learn a long the way


