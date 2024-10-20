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
1. Physical: things like data cables, Cat 6
2. Data switching: MAC addresses & NIC
3. Network: IP addresses, routing
4. Transport: TCP/UDP
5. Session: session management
6. Presentation: WMV, JPEG, MOV
7. Application: HTTP, SMTP

* We *receive* data down the physical layer all the way down to application
* We *transmit* data from the application layer all the way down to physical
* When troubleshooting, always start with Layer 1 (PHYSICAL) and work up to Layer 7 (APPLICATION)

## Hour 2
### Subnetting
* Use ifconfig to look at IP address
* A subnet mask is a 32-bit number used to determine which part of an IP address identifies the network and which part identifies the host within that network; these are written in dotted decimal notation
* CIDR (Classless interdomain routing) notation is a more comapct way to represent subnet masks; it is a slash with a number like /24, and the number is *how many bits in the subnet mask are set to 1*
 * /24 subnet mask has 24 bits set to 1, which is equivalent to 255.255.255.0 in dotted decimal notation
* The subnet masks determines the number of hosts that can be accomodated within a subnet
 * The number of host addresses is 2^number of host bits (32-CIDR number) - 2
 * We subtract 2 because the first address is reserved for the network ID and the last address is reserved for the broadcast address  
* Associate /24 with homes and small businesses because of the number of hosts it allows
 * A /24 subnet has 32 - 24 = 8 host bits, so it can accomodate 2^8 - 2 = 254 hosts
* /16 is for fairly large organizations and allows for 65,534 hosts
* /8 is for very large organizations and allows for millions of hosts
#### Why Subnetting Matters for Pentesting
* With IP address & CIDR, a pentester can determine the size of the network they are targeting; this helps plan the scope of the pentest
* Subnetting helps pentesters scan target networks efficiently by scanning only a specific subnet range instead of all possible IP addresses
* Subnetting can be used to segment networks, isolating different systems or departments
* Use ipaddressguide.com to calculate subnet details, including first and last IP addresses, the number of hosts, and the subnet mask

## Hour 3
* Working on installing VMWare Kali instead of VirtualBox
* Stopped at 1:55:39 of video, still need to unzip the VMWare Kali file
* Will need to re-do all of the UFSIT instructions for jumpbox and connecting to Sliver
* But that's fine because I hated VirtualBox
* https://notebooklm.google.com/notebook/b45d0154-fe34-4385-8026-277fbefc5f36?_gl=1*e7qvf1*_ga*MTEyNDc5NTczNC4xNzI5MTc2NTE1*_ga_W0LDH41ZCB*MTcyOTQ0NzYzNi4zLjEuMTcyOTQ0NzYzNi42MC4wLjA.
