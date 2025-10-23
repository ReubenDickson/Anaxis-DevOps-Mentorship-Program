 
6-Month DevOps Engineer Mentorship Program (Week-by-Week)

Week 1 Lecture Notes: Getting Started with Linux (Ubuntu)
Goal:
By the end of this week, students should:
•	Understand what Linux is and its components.
•	Know different methods to install and use Linux (dual boot, virtual machines, cloud, etc.).
•	Be comfortable navigating the Linux terminal.
•	Perform basic file and directory operations.
•	Use text editors (nano, vim) effectively.


What is Linux?

Linux is an open-source operating system based on the UNIX architecture. It manages hardware resources and provides an environment where users can run applications.

Key Components
•	Kernel: The core part that interacts with hardware (CPU, memory, disk).
•	Shell: The interface that allows users to type commands (e.g., Bash).
•	File System: Organizes how data is stored and accessed.
•	User Space: Where user applications and tools run.

Why Linux for DevOps
•	Most servers run on Linux.
•	DevOps tools (Docker, Kubernetes, Jenkins, Ansible) are built for Linux.
•	It’s secure, stable, and customizable.


Understanding Ubuntu
Ubuntu is one of the most beginner-friendly Linux distributions. It’s based on Debian and comes in two main editions:
•	Ubuntu Desktop: For personal use with a graphical interface (GUI).
•	Ubuntu Server: For production environments and cloud servers (no GUI).
For DevOps training, we’ll primarily use Ubuntu Server, but you can practice with Ubuntu Desktop or both.

Methods to Install or Use Linux
Depending on your computer setup, you can install or use Ubuntu in various ways.
A. Dual Boot Installation
Install Ubuntu alongside Windows or macOS.
•	Pros: Full access to your hardware, faster performance.
•	Cons: Must reboot to switch between OSes.
•	Steps:
1.	Download the Ubuntu ISO file from ubuntu.com/download.
2.	Create a bootable USB using Rufus (Windows) or Balena Etcher (cross-platform).
3.	Boot from USB (change boot device in BIOS).
4.	Follow prompts → Select “Install Ubuntu alongside Windows”.
5.	Reboot and choose OS during startup.

B. Using VirtualBox
Run Ubuntu inside a virtual machine without affecting your host OS.
Requirements:
•	Download and Install VirtualBox - https://www.virtualbox.org/wiki/Downloads
•	Minimum specs: 4GB RAM, 25GB storage free.
Steps:
1.	Open VirtualBox → “New” → Name it Ubuntu.
2.	Allocate 2GB+ RAM and 20GB+ disk.
3.	Attach Ubuntu ISO as a virtual CD.
4.	Start VM → Install Ubuntu normally.
5.	Once installed, you can start and stop Ubuntu anytime from VirtualBox.
Tip: Install “Guest Additions” in VirtualBox for better screen resolution and clipboard sharing.

C. Using WSL (Windows Subsystem for Linux)
For Windows 10/11 users.
Steps:
1.	Enable WSL:

```bash
$ wsl ---install
```

2.	Choose Ubuntu from the Microsoft Store.
3.	Open “Ubuntu” from Start Menu → set username/password.
4.	You can now use Linux commands in Windows Terminal.

Pros: Lightweight, fast, easy to set up.
Cons: Limited for advanced networking or Docker setups.

D. Using the Cloud (AWS, GCP, Azure, Linode, etc.) - Most Recommended
Launch Ubuntu Server in the cloud and access via SSH.
Example: AWS Free Tier
1.	Create an AWS account → Go to EC2 Dashboard.
2.	Launch instance → Choose Ubuntu Server 22.04 LTS.
3.	Select instance type (t2.micro for free tier).
4.	Generate & download key pair.
5.	Connect via SSH:
6.	ssh -i yourkey.pem ubuntu@<your-ec2-ip-address>

Pros: Real-world DevOps experience.
Cons: Requires internet and cloud billing awareness.

E. Online Linux Terminals (for Quick Start)
Use web-based Linux environments:
•	https://www.katacoda.com
•	https://labs.play-with-docker.com
•	https://canonical.com/lxd
Best for quick practice without installation.


Understanding the Linux File System
Linux organizes files in a hierarchical tree starting from the root directory /.

Directory	        Description
/	                Root directory
/home	            User home directories
/bin	            Essential binaries (commands like ls, cp)
/etc	            Configuration files
/var	            Logs, temporary data
/tmp	            Temporary files
/usr	            User applications and libraries
/root	            Home directory for the root user

Practice Exercise:
Open your terminal and explore:

``` bash
cd /
ls
cd /home
pwd
```

Navigating the Linux Command Line
Basic Commands

Command	        Description
pwd	            Show current directory
ls	            List files/folders
cd	            Change directory
clear       	Clear screen
man <command>	Show command manual

Example:

``` bash
cd /home
pwd
ls -l
```

File Operations

Command	                Description
touch file.txt  	    Create an empty file
mkdir myfolder	        Create directory
cp file1 file2  	    Copy file
mv file1 newfolder/ 	Move file
rm file.txt	            Delete file
rmdir folder	        Remove empty directory
Caution: rm -rf deletes recursively — use carefully.

Viewing and Editing Files

View Files
``` bash
cat file.txt      # Display content
less file.txt     # Scroll through long files
head -n 10 file.txt  # First 10 lines
tail -f /var/log/syslog  # Live log view
```

Edit Files
•	nano (easy for beginners)
•	nano file.txt
Save = Ctrl + O, Exit = Ctrl + X.
•	vim (powerful editor)
•	vim file.txt
o	Press i → Insert mode.
o	Type your text.
o	Press Esc, then :wq to save & quit.
Practice:
1.	Create a file about_me.txt
2.	Add your name, goals, and save using both nano and vim.


Understanding Command Syntax and Help
Linux commands generally follow this format:
command [options] [arguments]
Example:
ls -l /home
•	ls = command
•	-l = option (long format)
•	/home = argument (path)
To learn any command:
man ls
or
ls --help

Environment Variables
View and set environment variables:

``` bash
echo $HOME
export MYNAME=Anabel
echo $MYNAME
```

Hands-on Lab Exercises
1.	Create a file structure like:
2.	/home/student/projects/week1
3.	Inside week1, create files: bio.txt, goals.txt, todo.txt
4.	Write your daily goals into todo.txt.
5.	Copy todo.txt to /tmp.
6.	Compress week1 folder:
7.	tar -czvf week1.tar.gz week1
8.	Delete original folder, then extract back:
9.	tar -xzvf week1.tar.gz


Challenge Exercise
Write a small report (in report.txt) answering:
1.	What are three ways to install or use Ubuntu?
2.	What’s the difference between /home and /etc?
3.	How do you create, view, and edit a file in Linux?
Save it to your GitHub repo under:
linux_basics/week1/report.txt

Summary
By the end of Week 1, you should:
•	Understand what Linux is and why it’s used in DevOps.
•	Have Ubuntu installed (locally or on cloud).
•	Navigate the Linux filesystem confidently.
•	Create, move, and edit files and directories using terminal.