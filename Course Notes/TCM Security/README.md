# TCM Security Notes
## Hour 1 
### Public & Private IP Addresses
* Addresses with 192.168 are Class C and always private
* We pass all of our many private IP addresses for our devices at home through a public IP address with NAT
* A Class C address lets us have 254 hosts, but big businesses use Class A 10.0.0.0 addresses because it gives you like 16 million hosts
* You buy public addresses from an ISP
* Class B starts with 172.16
* So all of our devices on the 192.168 network communicate through one public address that the person bought from an ISP


first 3 groups of 2 of MAC address can be used to identify what the device is!!!! layer 2, related to switching

layer 4, transport: tcp is connection oriented protocol udp is connectionless like voice over up and dns

TCP has 3 way handshake, SYN, SYN ACK, ACK, Syn is wave hello, SYN ack is wave back, ack is knowing you can start a conversation now

To connect to website on port 443, you send out a SYN packet, if 443 is open they’ll send back a SYN ACK that says you can connect to them, and when you actually want to establish the connection YOU send an ack packet back

Wireshark is built into Kali Linux, listens in on NIC to capture data

Start capture, refresh google, and a bunch of data packets show up, you can see the 3 way handshake show up in wireshark

WRITE DOWN TCP VS UDP PROTOCOLS

FTP TCP 21, used a lot with CTF, means we can log into a server and get a file off the server or put a file on the server

Telnet is remote login to machine 23, SSH 22 is the encrypted version of Telnet

SMTP 25, POP3 110, IMAP 143 all relate to mail

DNS 53 resolves IP addresses to names

HTTP80 HTTPS 443

SMB 139 and 445, MOST COMMON AS A PENTESTER, Relate to file sharing and also referred to as Samba, a ton of exploits related too SMB like eternalblue that use SMB to navigate through a network

DNS 53 is also UDP

DHCP 67/68 associates you with an IP address using your layer 2 data to go to layer 3

TFTP on 69 for UDP FTP

SNMP 161 has community and public strings that can be useful

P physical data cables cat 6
Data switching, MAC addresses, NIC?
Network IP addresses, routing
Transport TCP/UDP
Session session management
Presentation WMV JPEG MOV
Application HTTP, SMTP

we RECEIVE data down the physical layer all the way down to application

we TRANSMIT data from application all the way down to physical 

When troubleshooting, always best to START with layer 1 physical and work up to application 7
```
code
```
