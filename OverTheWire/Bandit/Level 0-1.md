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
# Level 1
Goal: Find the password in a file called - located in the home directory
## Information
* Commands needed: ls, cd, cat, file, du find
* I know ls lists the files in a directory
* cd changes directory
* **cat outputs the contents of a file**
* **file shows the file type of a file**
* **
