# TCM Security Notes
## Hour 1

```
code
```
### Public and Private IP Addresses, NAT
* 192.168 is private Class C
* We pass private addresses through a public address with NAT
* A Class C address lets us have 254 hosts, and big businesses uses a Class A 10.10.0.0 address because it gives you like 16 million hosts
* You have to buy public addresses from an ISP
* Class B is around 172.16
* All devices on a 192.168 network communicate through one public address that the owner of the network bought from an ISP

### Packets & Such
* The first 3 groups of 2 of a MAC address can be used to identify what the device is
  * MAC is Layer 2 and related to switching
* Layer 4 is the transport layer
  * Remember that TCP is a connection-oriented protocol and UDP is a connectionless protocol that is used for things like voice over IP and DNS
  * TCP has a 3 way handshake: SYN, SYN ACK, ACK
    * SYN is the wave hello, SYN ACK is the wave back from your neighbor, and ACK is now knowing that you can start a conversation
  * To connect to a website on Port 443, you send out a SYN packet, and if 443 is open, they'll send back a SYN ACK that says you can connect to them, and when you actually want to establish the connection you send an ACK packet back
* Wireshark is built into Kali Linux and listens in on an NIC to capture data
  * You can start a capture, refresh Google, and you'll see the 3 way handshake show up in Wireshark

### TCP vs. UDP Protocols
#### TCP Protocols
* FTP is TCP 21: used a lot with CTF, means we can log into a server and get a file off the server or put a file on the server
* Telnet is TCP 23: remote login to a machine
* SSH is TCP 22: the encrypted version of Telnet
* SMTP 25, POP3 110, IMAP 143 TCP: all relate to mail
* DNS TCP/UDP: resolves IP addresses to names
* HTTP 80 & HTTPS 443 TCP: obviously used for web stuff
* SMB 139 & 445 TCP: MOST COMMONLY RELATED TO PENTESTING, relate to file sharing and also referred to as Samba; there are a ton of exploits related to SMB like EternalBlue that use SMB to navigate through a network
#### UDP Protocols
* DNS is UDP 53: DNS is also UDP
* DHCP is UDP 67/68: associates you with IP addresses using your Layer 2 data to go to Layer 3
* TFTP is UDP 69: UDP version of FTP
* SNMP is UDP 161: has community and public strings that can be useful

### OSI Model
#### Breakdown of Each Layer
1 Physical: things like data cables, Cat 6
2 Data switching: MAC addresses & NIC
3 Network: IP addresses, routing
4 Transport: TCP/UDP
5 Session: session management
6 Presentation: WMV, JPEG, MOV
7 Application: HTTP, SMTP

* We *receive* data down the physical layer all the way down to application
* We *transmit* data from the application layer all the way down to physical
* When troubleshooting, always start with Layer 1 (PHYSICAL) and work up to Layer 7 (APPLICATION)

