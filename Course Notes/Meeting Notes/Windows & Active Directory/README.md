# Windows & Active Directory
## Important File Paths
* C:\Users: where user data is
* C:\Program Files: files for things you install
* C:\System32: things for the operating system
* C:\Program Files (x86): 32 bit or x86 files
## Registry
* Custom file system in Windows where all settings are stored
* Database with configurations and environment variables
  * Can customize start menu, configure default applications and hardware settings, adjust TCP/IP settings and network shares, and adjust user permissions and system security
  * Use regedit to edit the registry
* HKEY: Handle to Keys
  * HKCU: Handle Key Current User: stores configurations specific to the user that is logged in
    * **Changes in HKCU take precedence over similar settings in HKLM**
    * The HKCU key is a pointer for the HKEY_USERS (HKU) key specific to the logged-in user
    * The hives of HKU's subkeys are stored at %USERPFOFILE%
  * HKLM: Handle Key Local Machine: stores system keys that apply to all users on the local computer
     * The hives of HKLM's subkeys are stored at %SYSTEMROOT%System32\config
  * Value types are DWORD or QWORD, 32/64 bit numbers
    * And *_SZ is a string
  * Edit registry with regedit.msc
## Command-Line Shells
* cmd.exe: almost everything used is an executable like whoami.exe, but has some internal commands
* powershell.exe: more modern with a scripting language; uses "cmdlets" that call .NET
* terminal: newer, lets you have multiple tabs with different shells
## Powershell.exe
* Commands are called "cmdlets"
* Supported on many operating systems
* Uses verb-noun syntax: Get-ChildItem (ls), Get-Content (cat), Invoke-Expression, Start-VM
* Highly integrated with the Windows API, so it can manage pretty much anything with it like Users, Services, Apps, and Registry Keys
* https://underthewire.tech/ to learn Powershell
## Active Directory
* Microsoft's Directory Service" for use in Windows domain networks
  * Also works with Linux and MacOS
* AD usually refers to Active Directory Domain Services (AD DS)
* Provides centralized and standard management of network "objects"
  * Like Users, Groups, Computers, Policies, etc.
