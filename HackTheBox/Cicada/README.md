## From Watching Adam's Demo
* Used TTL, SMB, netexec, made list of users and used password spraying with netexec again to try the password with all users on the Cicada domain
* Changed the etc/hosts file to add an IP address and then the FQDN for the Cicada domain and then the Cicada domain
* Used guest account to get into SMB share, and in the SMB share there was a password that we used for the password spraying
* Command for changing clock skew is sudo ntpdate us.pool.ntp.org, Kerberos won't work correctly if there is clock skew
* Netexec can be used instead of nmap because it's easier
* Need to figure out how to use pwnbox

  ```
  pipx
  apt install netexec
  impacket
  smbclient
  smbclient.py (one of those two is the one you're supposed to use)
  pipx install git+(repository name) to install netexec
  if you want a nice impacket version, use the github one from "theporgs"
  # use HR
  # ls
  bloodhound-python
  ```
* Should probably learn Linux shortcuts for copying and pasting IP addresses & filtering through past commands
* Use tldr with a command to see the most common usages
* Bloodhound Python might show you the shortest path to a goal user you want to access - this sounds like a helpful tool to install
* 

