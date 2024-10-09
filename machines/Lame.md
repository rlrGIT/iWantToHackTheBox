- Attempting in guided mode

### Task 1
- How many of the top 1000 TCP ports are open?

I am not getting a clear view of the number of open ports, but I am getting a certain number of filtered ports. I believe my requests are being ignored, but I am yet to confirm this.

### Direct Machine Ping
`ping <ip>`
- "Destination Host Unreachable"
Should disable ICMP echoes.

--------
### Browser Lookup
- Connection has timed out


-----
### Pure SYN Scan Results

`sudo nmap <ip> --top-ports 1000 -n -Pn --disable-arp-ping` should give us a view of how the SYN scan is working without interference from DNS, ICMP echoes, and ARP ping. But after doing this, we only get information that 84 ports of the top 1000 filtered.

**Notes:
- `-Pn` because we do not need host discovery, and we do not want to send out meaningless packets if 
- `--disable-arp-ping` because
- `-n` was an optimization, might be worth running with DNS?
	- This made no difference, but I do see 88 filtered without using this option, not sure what this means...

**Theories:
- Probably some form of firewall, because have a lot of filtered and non-responsive. Looking into firewall evasion...

-------
### Firewall Tactics

I am jumping back to the basics starting with the workflow described by the Nmap module.

- Host Discovery
- Saving the Results
- Service Enumeration
- Scripts
- Firewall/IDS/IPS Evasion

--------
### Host Discovery / Nmap definitions
- We can leverage time-to-live `(ttl)` values to try and identify the host system by examining how individual packets are returned with `--packet-trace`

| **State**          | **Description**                                                                                                                                                                                         |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `open`             | This indicates that the connection to the scanned port has been established. These connections can be **TCP connections**, **UDP datagrams** as well as **SCTP associations**.                          |
| `closed`           | When the port is shown as closed, the TCP protocol indicates that the packet we received back contains an `RST` flag. This scanning method can also be used to determine if our target is alive or not. |
| `filtered`         | Nmap cannot correctly identify whether the scanned port is open or closed because either no response is returned from the target for the port or we get an error code from the target.                  |
| `unfiltered`       | This state of a port only occurs during the **TCP-ACK** scan and means that the port is accessible, but it cannot be determined whether it is open or closed.                                           |
| `open\|filtered`   | If we do not get a response for a specific port, `Nmap` will set it to that state. This indicates that a firewall or packet filter may protect the port.                                                |
| `closed\|filtered` | This state only occurs in the **IP ID idle** scans and indicates that it was impossible to determine if the scanned port is closed or filtered by a firewall.                                           |
### Got connection to port 1434 over UDP with a -sC script scan
- Was able to use a script scan `-sC` to find that there is an ms-sql-m service on port 1434, but the port is still filtered. 
- Now we know that we have a probably Linux Machine running an ms-sql-m service

### Attempting timing based information gathering seeing if there is a difference between -sT and -sS calls, indicating firewall activity

```
sudo nmap <ip> --packet-trace --disable-arp-ping -Pn -n --reason -sT -p 80
```

```
sudo nmap <ip> --packet-trace --disable-arp-ping -Pn -n --reason -sS -p 80
```

- Maybe good in the future to determine timing differences between different types of scans...
- SYN (-sS) scan finishes almost instantaneously, and connect scan (-sT) takes 2 seconds... I'm guessing this is firewall, but the behavior is the opposite of what we see in the labs.

### UDP scan yielded all unreachable
```
sudo nmap 10.129.40.132 -n -Pn --disable-arp-ping --packet-trace --top-ports 1000 --reason -sU
```

### Port 53 (DNS) is open|filtered when scanning with -sC directly

### Port 1434 is open|filtered when scanning with -sC and -sU directly

- At this point, pretty sure there is some IDS/IPS/Firewall
Will counting all the open|filtered ports count?

### Using -sU does yield more open|filtered results

### Craft a packet from the open ms-sql-m service?

--source-port 53 does not work, but there could be others?
We are basically troubleshooting to find the firewall rules at this point.

### Fragmenting packets (-f) yields filtered flag

### I WAS CONNECTED TO THE WRONG VPN
