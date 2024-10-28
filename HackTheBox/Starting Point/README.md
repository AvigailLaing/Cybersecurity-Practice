# Meow
* Tried sudo openvpn /home/avigail/Downloads/path_to_file.ovpn
  * This is done when you see an "initialization sequence completed"
```
user: avigail
passwd: KaliLinux or kalilinux
```
* Got on VPN, pinged target IP, worked
## Introduction
* Enumeration - documenting current state of the target machine to learn as much as possible about it
  * This should be your first step right now
  * If target machine is a web server, you can go to that IP address to see what the web page contains
  * If the target is a storage server, you can connect to it using the same IP address to explore the files and folders on it (if you have the right credentials)
  * **First step is always to scan all of the open ports to see the purpose of the target on the network and what potential vulnerabilities might appear and can be exploited from the services running on it; use nmap to scan for ports quickly**
    * After seeing which ports are open, manually access each port using different tools to find out if we have access to their contents or not
```
sudo nmap -sV {target ip}
```
* The -sV is a service detection flag that will show you the name and description of the identified services
* telnet is open (23), since our target is running telnet, it can receive telnet requests from other hosts in the network; connection requests through telnet are configured with user/password combinations
  * Some accounts have blank passwords for the sake of accessibility, many accounts have usernames like admin, administrator, root
  * Try logging in with these credentials to see if they're blank
  * You can also use wordlists for usernames and passwords with a script to automate the process (typical people names, abbreviations, data from database leaks)
  *  do telnet {target ip} and then it'll take you to the login page, then try admin, administrator, and root
    * root will get you into the system, and from here, we can run commands at root
      * running the ls command will show you the files on the root user - this shows flag.txt and snap
        * cat flag.txt reveals a flag
# Fawn
* nmap -sV {target ip} reveals that ftp (21) is open; now let's try to use ftp to get more information
```
ftp 10.129.174.155
```
* Now I am connected to the ftp server
  * FTP can transfer log files from one network device to another or a log collection server
  * Many times FTP is not properly secured, allowing us to gain leverage of the logs and extract information from them that helps us map out the network, enumerate usernames, detect active services, and more
  * Users download/upload files from the client (their own host) to the server (a centralized data storage device)
  * Client is always the host that downloads and uploads files to the server, server is always the host that safely stores the data being transferred
  * Clients can browse available files on the server using the FTP protocol; FTP traffic contained files can be intercepted with a MitM attack, and contents of files can be read in plaintext; if network admins tunnel FTP through SSH or wrap the connection with SSL/TLS, the encryption would foil MitM attacks
```
ftp -?
```
* ftp -? can be used to see what the service is capable of
* ftp is often misconfigured to have usernames like anonymous able to access the service; when it asks for the password, **any password can be used, you can literally type in anything and still get access**
* so enter username anonymous, password anything
* privesc part of TCM course, prewrite by looking at past CPTC reports, do tryhackme for privilege escalation specifically
* do help command to see what FTP commands are available, then do man {command} to see more specific information about a command
* **always try ls with ftp**
  * whoami, host ID should be visible in screenshots for CPTC
  * once you see something you want to download like a .txt, do get flag.txt to download it to your computer; it downloads, the file to the same directory you were in when you issued the ftp command
  * do "quit" to exit the ftp terminal
