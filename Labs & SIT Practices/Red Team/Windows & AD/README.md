# Windows & AD
* netexec can do everything
* install with sudo apt install netexec
  * then sudo apt install impacket
* notes in order of pentest process
### Network Recon
* nmap scan on specific subnet (nmap -sC -sV)
* run nxc smb 10.10.10.0/24: SMB often gives away domain name
  * now add an IP address and domain name to the hosts/etc file
* EternalBlue is an exploit you can run with no credentials to get administrative privileges
  * very unstable, don't recommend using in a pentest
  * can get you NT\AuthoritySystem
* ZeroLogon is another vulnerability; lets you bruteforce an AES key that allows you to call a function to change password of domain admin
  * Will break whole DC unless you're careful
* when you get on a network, there are 2 types of login you should try unless you got credentials
  * anonymous login is when you have any username and no password, it lets you view shares and view users (via rid-brute)
  * null login lets you view shares, users, groups, password policy (rarer)
  * this has to do with the smb command on nxc
* use smbclient.py to log in with username, password, and ip address, and then you can use ls to look at shares
* first check password policy with --pass-pol, then do password spraying
  * try no password, try username=password
  * generate custom wordlist for password cracking: https://github.com/p0dalirius/LDAPWordlistHarvester
    * you can come up with wordlists based on regions by using IPs 
  * you can also use --local-auth for local account login
* rid brute used with nxc smb ip -u 'a' -p '' --rid-brute
  * 'a' can be any letter, doesn't matter
  * you can always do this with anonymous login
  * this shows you the usernames
* null authentication with nxc smb hosts.list -u '' -p --users
  * this shows a username and a description; the description often reveals passwords
* smb allows you to execute code
  * if it said Pwn3d, that means you have local admin & can execute code
* LLMNR is basically DNS; if computer is looking for a file share that doesn't exist, it requests to every other computer on the domain where the share is, and they can respond with an IP address...even fake ones, so you can respond with your own IP
  * if you make a fake SMB, you receive NetNTLM hashes from people who try to get the file share; these do require cracking
  * this is enabled by default on Windows
* more about relaying
  * you can capture a hash by getting people to browse to your SMB share
    * p0dalirius/Coercer
  * or getting a database to request a file on your SMB share
  * or sending an email with an image on your SMB share
    * this is why Outlook scans things before loading an image 
  * or making a website request a file on your SMB share
  * relaying is capturing a hash AND forward it to another host to authenticate; nxc smb <IP or subnet> --gen-relay-list relayable.txt
    * these are machines you can trick, we're using nxc to get a list of people with signing disabled
* slinky module: you can put an image in a LNK file in a writeable SMB share
  * every time someone opens a folder, it'll try to preview an image and authenticate
* exploits for coercing authentication
  * PetitPotam allows you to coerce a Windows host to authenticate to other hosts
  * ShadowCoerce is entirely over SMB, exploits MS-RSVP
* Coercer program goes through every single common exploit to try to coerce authentication; it does all the work for you
* you can use the responder tool to receive hashes
  * every time someone authenticates to you, you get the hash
  * you can copy it over to a file and try to crack it to get a password
  * you will never crack a machine account (name will end with $) hash because they're regenerated every month; machine accounts for connecting to domain and doing things related to system
    * you can crack a user account though
  * machine accounts are domain accounts, system accounts for services are local
* how to relay hashes
  * step 1: generate relay list by checking where smb signing is disabled; on the blue team side, you should make sure smb signing is enabled
    * nxc smb hosts.list --gen-relay-list tf.txt
  * step 2: set up a listener to relay hashes to relayable machines
    * ntmrelayx.py -smb2support -tf tf.txt
  * step 3: coerce authentication in some way (see previous notes)
* if you ever get an NTLM hash, you can use it directly to login
* exploits that need credentials
  * PrintNightmare: lets you use print spooler (RPC) to remotely add printer drivers on the system as admin, letting you execute code as admin and become admin
    * netexec will help you scan this
  * noPac is a logic bug that allows you to become domain admin with any user account
  * noPac.py will allow you to impersonate any user you want
* ldap things
  * if you're a user, you can scan ldap and look at all attributes
    * ldapsearch gives you a domain like DC-snaplabs,, DC=local
    * ldapsearch with bash magic can sort by frequency of a string and show you what's more unique; can give list of users
    * impacket tool called GetADUsers.py will get a list of all users
    * nxc ldap with get desc-users can show users with descriptions
      * if you get a default password, try it on all users if lockout policy is chill enough
      * password spraying is good because you only use one logon on every user (so only 1/3 lockouts, shouldn't trigger locking out every user)
      * but the blue team can detect this if they see a certain IP trying to log into every user or if 100 failed logins show up
* bloodhound tool grabs all users in active directory and creates a graph with nodes to show privilege escalation paths
  * you can use graph theory to find the shortest path from one user to another
  * this by default makes an unweighted graph but some tools can make a weighted graph that show which paths are easier to execute
    * sometimes a path of 10 is easier than a path of 3 if a path of 3 requires you to exploit things
  * do not do this anywhere real unless you have permission
  * you can do it on hackthebox though
  * if you're on a local machine, you can use sharphound.exe to collect all data for local domain then upload to bloodhound
  * bloodhound.py with user and password can also be used to remotely collect all the info
* dangerous privileges
  * GenericAll gives you full rights to the object (can add users to group or resut user's password)
  * GenericWrite: update object's attributes (ex. logon script)
  * WriteOwner: change owner
  * WriteDACL: change access control list
  * AllExtendedRights: add user to group or reset password
  * ForceChangePassword: change user's password
  * DCSync: if you change password, you might be able to log in with old and new password because old and new domains might have different passwords
    * DCSync can be used to get credentials from a domain controller; could give you everyone's password, including the admin's
* kerberos
  * ASReproasting: issue for all users with no pre-authentication; the same thing as Kerberoasting, but with users instead of service accounts
    * impacket-GetNPUsers.py -request
      * NPUsers means no pre-auth users
    * you can make it so that no pre-authentication is required on an account
  * Kerberoasting
    * can request hash of a service account for free in kerberos; if a service account has a SPN (Service Principal Name), you can request it and get dangerous information, even without credentials on the domain
    * impacket-GetUserSPNs -request
  * kerberos only works if clock is synced with DC
    * sudo ntpdate <DC IP>; run to sync clock with domain controller
  * bloodhound can help you get all accounts that you can use ASReproast or Kerberoast with...i think
* ACDS
  * active directory certificate services
  * implements public key infrastructure
  * certificates & certificate templates can be used to access resources
  * microsoft advice for setting this up resulted in 8 exploits, some letting you get admin with no credentials
  * certipy is a great tool for performing ACDS recon
    * certipy find -u <u>@<domain> -p <p> -vulnerable
  * a certificate template is a blueprint of settings, options, and permissions that can be specified when generating a certificate
    * enrollment permissions specify who can request a certificate with the template (this could mean anyone can become domain admin)
    * pkiextendedkeyusage specifies what the certificate can be used for
* SCCM
  * system center configuration manager: manages task automation, remote control, and OS deployment
  * many vulnerabilities allowing for stealing domain credentials, taking over site servers (what can control everyone's computer), coercing authentication
  * thehacker.recipes has SCCM information
  * sharpSCCM allows you to do SCCM exploitation
* web
  * IIS RCE: sometimes you can write files to the app's webroot
    * if IIS is very old, there might be remote code execution
    * if you can write an executable file to the app, you can get execution on the app
    * you can write code, put it in the root of a website, browse to it, and then execute whatever you want
  * otherwise, try common web exploits like SQL injection, code injection, mssql injection if mssql is running
    * or you can coerce auth from a database or webpage if it has LFI
    * sometimes you might be able to guess usernames/passwords from a website
* MSSQL
  * if you have sql, sometimes you can request a remote file from mssql to coerce authentication, like with a malicious SMB server
  * sometimes you can execute commands
    * xp_cmdshell
  * sometimes, there is confidential information in the database
* RDP
  * xfreeRDP can be helpful
* winrm lets you get powershell
  * evil-winrm with username and password gives you access to powershell
  * can let you login with pass the hash
* post-exploitation
  * when you get onto a machine
  * password dumping: mimikatz.exe, impacketsecretsdump.py
  * tryhackme for postexploit, can help you get higher privileges
* local privilege escalation
  * whoami /priv
* dangerous privileges
  * seinstallalwayselevated will always run things as administrator; so you can run any code you want as administrator
  * sedebugprivilege lets you debug process memory, meaning you can dump LSA secrets (domain creds)
  * seimpersonate lets you impersonate another client, usually means easy privesc through potato attacks
    * service accounts often have this so that they can impersonate other users to work correctly; this can be used to impersonate any user including SYSTEM 
  * sebackupprivilege/serestoreprivilege lets you read/write any files, read the nts.dit file and dump something
* potato attacks: trick a process to run as an administrator and do things for you
### Good Resources
* TCM Security AD Course
* Netexec Wiki
* THM Enterprise Room
* Game of Active Directory
* TheHackerRecipes
* VulnLab
