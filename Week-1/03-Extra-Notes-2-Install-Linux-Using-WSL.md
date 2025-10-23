 
6-Month Junior DevOps Engineer Mentorship Program (Week-by-Week)

Week 1 Extra Notes 2: Installing Ubuntu Server on WSL
So, you’re a Windows user and thinking of running Linux on your PC without tampering with your Windows installation? Here’s the solution you need – Windows Subsystem for Linux (WSL). This detailed guide contains the step-by-step process for setting up WSL. Have fun!

What you will learn:
•	What WSL is and why use it.
•	Differences between WSL1 and WSL2.
•	Step-by-step installation of WSL and Ubuntu Server (Ubuntu on WSL).
•	Basic Linux server setup: users, SSH, packages, firewall.
•	Managing services (Nginx, MariaDB), file sharing, backups, and exports.
•	Troubleshooting common errors.
•	Hands-on exercises and a command cheat sheet.


Table of Contents
1.	Introduction: Why run Ubuntu Server on WSL?
2.	Requirements and prerequisites.
3.	WSL concepts — WSL1 vs WSL2.
4.	Preparing Windows: updates, virtualization, and PowerShell.
5.	Installing WSL and setting WSL2 as default.
6.	First boot: create user, update system, install essential tools.
7.	Basic server configuration (SSH, sudo, timezones, locales).
8.	Networking and how WSL interacts with Windows.
9.	Managing packages and services (systemd caveats and workarounds).
10.	Files, file permissions, and sharing files between Windows and WSL.
11.	Backups, exporting/importing your WSL distro.
12.	Security hardening basics for a beginner.
13.	Troubleshooting guide: common errors and fixes.
14.	Appendices: command cheat sheet, useful links, glossary.


1. Introduction: Why run Ubuntu Server on WSL?
Running Ubuntu Server inside Windows using WSL gives you a lightweight Linux environment for development, testing, learning server administration, and running Linux-native tools without needing a separate physical machine or full virtual machine. WSL2 provides a real Linux kernel and high compatibility while remaining integrated with Windows.
Use cases
•	Learning Linux server administration.
•	Developing web applications locally with the same stack that will run on production Linux servers.
•	Running CLI tools and package managers available only on Linux.
•	Quick test environments for services like Nginx, PostgreSQL, Redis.


2. Requirements and prerequisites
Before starting, ensure you have:
•	A Windows 10 (2004+) or Windows 11 system. For WSL2 you need a Windows build that supports it (Windows 10 2004+ with KB5004296 or later is generally OK). If you're unsure, check Settings > System > About.
•	64-bit Windows and CPU with virtualization support (Intel VT-x or AMD-V). Most modern machines have this.
•	Administrative access on your Windows user account (you'll need to run PowerShell as Administrator for some steps).
•	Internet connection to download packages and the Ubuntu image.
Tip: If you use Windows 11, WSL installation is easier because Microsoft provides a single wsl --install command that can do most steps for you.


3. WSL concepts — WSL1 vs WSL2
•	WSL1 translates Linux syscalls to Windows. It is lightweight but has compatibility limitations (e.g., less compatibility with Docker and kernel-specific features).
•	WSL2 runs a real Linux kernel inside a lightweight utility VM. Better compatibility, full system call support, and improved filesystem performance for many operations.
Recommendation for beginners: Use WSL2 unless you have a legacy reason to use WSL1.


4. Preparing Windows: updates, virtualization, and PowerShell
1.	Check Windows version
o	Open Settings > System > About. Note the OS build and version. If it's older than Windows 10 2004, update Windows.
2.	Enable Virtualization in BIOS/UEFI
o	Restart your PC and enter BIOS/UEFI (often by pressing Esc, F2, F10, Del depending on your machine).
o	Find CPU virtualization settings (Intel VT-x, AMD-V) and ensure they are enabled. Save and reboot.
3.	Open PowerShell as Administrator
o	Press Start, type powershell, right-click Windows PowerShell, and choose Run as administrator.


5. Installing WSL and setting WSL2 as default
There are two main ways: the simple wsl --install (Windows 10/11 recent builds) and the manual steps. We'll show both.
The easy way (Windows 11 or updated Windows 10)
    1.	In an Administrator PowerShell, run:

``` bash
wsl --install
```

This command enables the required features, downloads the latest Linux kernel, sets WSL2 as the default, and installs Ubuntu (default). Reboot when prompted.
    2.	After reboot, Windows will finish installing. Then launch the Ubuntu app from the Start menu.



6. First boot: create user, update system, install essential tools
When you first launch Ubuntu from the Start menu, it will run an initialization and then prompt you to create a UNIX username and password. These are separate from your Windows credentials.
    1.	Create username and password when prompted. Remember this username.
    2.	After the prompt, update packages:

``` bash
sudo apt update && sudo apt upgrade -y
```

    3.	Install common essential packages:
    
 ``` bash
        sudo apt install -y build-essential curl wget git ca-certificates apt-transport-https gnupg lsb-release nzip nano less
```
The above command installs the following packages; build-essential, curl, wget, git, ca-certificates, apt-transport-https, gnupg, lsb-release, unzip, nano and less.
    4.	Verify kernel/version info:
``` bash
        uname -a
        lsb_release -a
        cat /etc/os-release
```


7. Basic server configuration (SSH, sudo, timezones, locales)
Configure sudo and user
By default, the user you created will have sudo privileges. To add more users:

``` bash
sudo adduser newuser
sudo usermod -aG sudo newuser
Set timezone
sudo timedatectl set-timezone Africa/Lagos  # change to your timezone
```

Configure locales (if needed)
``` bash
sudo dpkg-reconfigure locales
Install and enable SSH server (openssh-server)
```
WSL doesn't automatically run systemd (unless configured); however, you can still run sshd manually or via a workaround tool. Steps below show starting sshd manually and an optional service wrapper.
    1.	Install openssh-server:
``` bash
sudo apt install -y openssh-server
```
    2.	Edit /etc/ssh/sshd_config to set PasswordAuthentication yes (not recommended for production), or better set up SSH keys. You must have learnt how to edit a file using the nano or vim command.
    3.	Start sshd:
``` bash
sudo /usr/sbin/sshd
```
    4.	Confirm sshd running:
``` bash
sudo ss -tlnp | grep ssh
```
Note on systemd: Historically WSL did not have systemd; recent Windows updates and WSL versions support systemd in WSL, which allows services to start normally. If your WSL supports systemd, you can enable it in /etc/wsl.conf or by setting WSL to support systemd (see later section).

Assignment: Take out time to do a quick search on ssh and systemd in Linux. Share what you learn on Linkedln and mention Reuben Dickson


8. Networking and how WSL interacts with Windows
•	WSL2 runs inside a lightweight VM with its own IP address. Windows and WSL can communicate via localhost for many use cases, but the WSL2 VM IP may change after reboot.
•	To access services on WSL from Windows: often http://localhost:PORT works because WSL forwards ports. If not, use the WSL IP from ip addr inside WSL.
ip addr show eth0
•	To access Windows files inside WSL: Windows drives are mounted under /mnt, e.g., /mnt/c/Users/Anaxis/Documents.
•	To access WSL files from Windows Explorer: open \wsl$\Ubuntu-22.04 or type wsl$ in Explorer address bar.


9. Files, file permissions, and sharing files between Windows and WSL
•	Linux file system is stored in a virtual disk (ext4.vhdx) and accessible via \wsl$\DistroName in Explorer.
•	Editing Linux files directly from Windows tools can cause permission issues—best practice: keep project code in Windows file system for Windows tools (/mnt/c/...) when using editors like VS Code; or use Linux-native editors when working in ~.
Permissions example:
# set executable
chmod +x script.sh
# set ownership
sudo chown -R username:username /some/path


10. Backups, exporting/importing your WSL distro
Use wsl --export and wsl --import for backups and migration.
Export:
wsl --export Ubuntu-22.04 C:\backups\ubuntu-2204-backup.tar
Import to new distro name:
wsl --import UbuntuRestore C:\WSL\UbuntuRestore C:\backups\ubuntu-2204-backup.tar --version 2
You can also use wsl --terminate <distro> and wsl --shutdown when performing maintenance.


11. Security hardening basics for a beginner
•	Use SSH keys instead of password authentication.
•	Keep system updated (sudo apt update && sudo apt upgrade -y).
•	Use ufw ( uncomplicated firewall ) to limit ports:
``` bash
sudo apt install -y ufw
sudo ufw allow openssh
sudo ufw allow 'Nginx Full'   # if using nginx
sudo ufw enable
```
•	Avoid running services as root. Create service users when possible.
•	Limit Windows–WSL file interactions for sensitive files.


12. Troubleshooting guide: common errors and fixes
Problem: WSL command not found or wsl --install fails
•	Ensure you're running PowerShell as Administrator.
•	Update Windows to latest patches.
•	Enable virtualization in BIOS.
Problem: Distro stuck at Installing, this may take a few minutes...
•	Check Task Manager for resource usage.
•	Try wsl --terminate <distro> and restart the distro.
•	If corrupted, export and re-import or uninstall and reinstall distro.
Problem: apt is locked or package installation fails
``` bash
sudo rm /var/lib/dpkg/lock-frontend
sudo dpkg --configure -a
sudo apt update
```
Problem: Networking issues — can't reach service from Windows
•	Check if service is listening: ss -tlnp inside WSL
•	If service binds to 127.0.0.1 it may only listen on the WSL VM. Use 0.0.0.0 to listen on all interfaces.
Problem: systemctl doesn't work
•	Your WSL may not have systemd. Use service <name> start or sudo /usr/sbin/<daemon> or enable systemd if supported.


13. Appendices
Command cheat sheet
# Update & upgrade
``` bash
sudo apt update && sudo apt upgrade -y
```
# Add user and give sudo
``` bash
sudo adduser myuser
sudo usermod -aG sudo myuser
```
# SS
``` bash
sudo apt install -y openssh-server
sudo /usr/sbin/sshd
```
# Export/Import WSL
# From PowerShell (Admin)
``` bash
wsl --export Ubuntu-22.04 C:\backups\ubuntu2204.tar
wsl --import MyUbuntu C:\WSL\MyUbuntu C:\backups\ubuntu2204.tar --version 2
Note that each line above is a different command
```
# Check kernel
``` bash
uname -a
```
# Shutdown WSL
``` bash
wsl --shutdown
```

Useful links and references
•	Official WSL docs: https://learn.microsoft.com/windows/wsl
•	Ubuntu on WSL: search Ubuntu in Microsoft Store
Glossary
•	WSL: Windows Subsystem for Linux
•	Daemon: Background service
•	systemd: System and service manager on many Linux distributions
•	VHDX: Virtual hard disk file used by WSL2 to store a distro

Final notes and next steps
This eBook gives you a full beginner-friendly, step-by-step guide to installing and using Ubuntu Server on WSL. You can follow chapters sequentially, don’t copy-paste, rather type the commands into your WSL terminal, and practice with the exercises.