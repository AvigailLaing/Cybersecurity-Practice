# Level 0
Goal: Log into the game using SSH
## Information
* Connect to host bandit.labs.overthewire.org on port 2220
* Username: bandit0
* Password: bandit0
* Commands needed: ssh
## Steps
```
$ ssh bandit0@bandit.labs.overthewire.org -p 2220
bandit0@bandit.labs.overthewire.org's password:
bandit0
```
**Command: ssh username@host -p [port number]**
* Default port number is 22, but if they specify a port, you need to do -p
# Level 1
Goal: Find the password in a file called readme located in the home directory and use it to log into bandit1
## Information
* Commands needed: ls, cd, cat, file, du find
* I know ls lists the files in a directory
* cd changes directory
* **cat outputs the contents of a file**
* **file shows the file type of a file**
* Once you find the file, you need to type "exit" to quit the current SSH session before joining a new one
* **Super useful, use the up arrow to filter through past commands so that you don't have to retype everything**
# Level 2
Goal: Find password in a file called - located in home directory
**find /home/bandit1 -name -**
**cat /home/bandit1/-**
* Logged out of ssh again with exit to log into the bandit2 account with this new password
# Level 3
Goal: Find password in a file called spaces in this filename located in the home directory
**cat /home/bandit2/"spaces in this filename"**
You have to put quotes around the filename if it has spaces around it
# Level 4
Goal: Find password in a hidden file in the inhere directory
