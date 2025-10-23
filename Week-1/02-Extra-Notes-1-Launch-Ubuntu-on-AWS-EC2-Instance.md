 
6-Month Junior DevOps Engineer Mentorship Program (Week-by-Week)

Week 1 Extra Note 1: Launching and Connecting to Ubuntu on AWS EC2
This e-book is prepared for absolute beginners and aspiring DevOps engineers who want to launch and connect to a cloud-based Ubuntu server on AWS EC2 using Windows or Linux.

What you'll learn:
•	How to sign up for an AWS account safely
•	AWS Free Tier basics and cost control
•	Key AWS concepts: Region, Availability Zone, VPC, Security Group, Key Pair, AMI, Instance Type
•	Step-by-step: Launch an Ubuntu EC2 instance
•	How to connect to the instance from Windows (PowerShell, PuTTY) and Linux/macOS
•	Using Session Manager (SSM) as an alternative to SSH
•	Post-launch tasks: update system, create users, configure firewall, enable automatic updates
•	Snapshot, AMI creation, and safe termination
•	Best practices and security checklist
•	Practical exercises and command cheat sheet

Table of Contents
1.	Introduction: Why use EC2 for learning Linux
2.	Prerequisites and safety tips
3.	Create an AWS account — step-by-step
4.	AWS Free Tier & billing alerts
5.	Key AWS concepts explained simply
6.	IAM: Create a secure admin user (do not use root)
7.	Launching an Ubuntu EC2 instance (console walkthrough)
8.	Connecting to your instance from Linux/macOS (SSH)
9.	Connecting from Windows (PowerShell OpenSSH)
10.	Connecting from Windows using PuTTY (key conversion)
11.	Using AWS EC2 Instance Connect and Session Manager (SSM)
12.	First steps on the server: updates, users, SSH hardening
13.	Installing common packages and setting up environment
14.	Creating snapshots and AMIs (backups)
15.	Cost management & how to avoid surprises
16.	Stopping, starting, and terminating instances
17.	Troubleshooting common connection issues
18.	Practice projects and exercises
19.	Command cheat sheet and appendix


1. Introduction: Why use EC2 for learning Linux?
AWS EC2 (Elastic Compute Cloud) lets you rent virtual servers in minutes. For a beginner, EC2 provides a real, internet-accessible Linux server you can practice on without buying hardware. It closely mirrors production servers, so skills transfer directly into DevOps and sysadmin roles.


2. Prerequisites and safety tips
Before you begin:
•	A current email address and a credit/debit card (AWS requires a card for verification). You will not be charged if you stay within the Free Tier, but the card is required.
•	A laptop or desktop running Windows, macOS, or Linux.
•	Basic comfort with the command line (we'll include commands and copy-paste examples).

Safety tips:
•	Create an AWS account with your own email and enable MFA (multi-factor authentication) on the root account immediately.
•	Do not use your root account for daily tasks — create an IAM admin user instead.
•	When creating security groups (firewalls), only open ports you need. For SSH, restrict access to your IP.
•	Stop or terminate instances when not in use to avoid charges. Use tags to track resources.


3. Create an AWS account — step-by-step
    1.	Open your web browser and visit https://aws.amazon.com/.
    2.	Click Create account.
    3.	Enter your email, password, and account name. Choose "Personal" or "Professional" based on your use.
    4.	Provide contact information (name, phone, address).
    5.	Enter your payment details (credit/debit card). AWS will perform a small authorization charge to verify the card — it is temporary.
    6.	Verify your phone number by entering the code from SMS or voice.
    7.	Choose a support plan (Basic is free and fine for learning).
    8.	Wait for the account to become active — this may take a few minutes.
    9.	After activation, sign in to the AWS Management Console (console.aws.amazon.com).

Enable MFA on root account
•	In the console, go to My Account or AWS Management Console > IAM > Users, and follow instructions to enable MFA for your root user.


4. AWS Free Tier & billing alerts
AWS Free Tier provides limited free usage for many services (for example, 750 hours per month of t2.micro or t3.micro Linux instances for 12 months). To avoid unexpected charges:
•	In the Billing console, set up a billing alarm in CloudWatch to notify you if monthly charges exceed a small threshold (e.g., $1).
•	Regularly check the Billing dashboard.
•	Always stop or terminate instances you don't need (stopped instances may still incur EBS storage costs).


5. Key AWS concepts explained simply
•	Region: A geographic area (e.g., us-east-1). Choose a region close to you for lower latency and legal reasons.
•	Availability Zone (AZ): Isolated data centers within a region.
•	AMI (Amazon Machine Image): A template for the instance (OS + initial software). We'll use an Ubuntu AMI.
•	Instance Type: Virtual hardware (CPU, memory). Free Tier eligible types include t2.micro and t3.micro.
•	Key Pair: An SSH key pair used to authenticate to your instance.
•	Security Group: Acts like a virtual firewall controlling inbound/outbound traffic.
•	EBS (Elastic Block Store): The virtual disk for your instance.
•	IAM (Identity and Access Management): Manage users and permissions — don't use root for daily tasks.


6. IAM: Create a secure admin user (do not use root)
    1.	Sign in to the AWS Console as the root user.
    2.	Open IAM (search for IAM in the console).
    3.	Click Users > Add user.
    4.	Enter a username (e.g., admin-user).
    5.	Select Access type: check AWS Management Console access and set a custom password. Optionally add programmatic access if you'll use the AWS CLI.
    6.	Attach existing policies directly: choose AdministratorAccess (this grants full rights — for learning it's okay, but in production use least privilege).
    7.	Finish creating the user and save the login link and credentials.
    8.	Sign out of root and sign in as your new IAM user. Enable MFA for this user too.


7. Launching an Ubuntu EC2 instance (console walkthrough)
Step-by-step
    1.	In the AWS Console, go to EC2 > Instances > Launch Instances.
    2.	Name and tags: Give your instance a name like ubuntu-learning-01.
    3.	Application and OS images (Amazon Machine Image - AMI): Search for Ubuntu and select an official Ubuntu Server LTS (e.g., Ubuntu Server 22.04 LTS provided by Canonical). Make sure it is an Ubuntu AMI.
    4.	Choose an Instance Type: Choose t2.micro or t3.micro (Free Tier eligible). Click Next.
    5.	Key pair (login): Create a new key pair or choose an existing one.
If creating a new key pair: enter a name (e.g., ubuntu-ec2-key), select RSA and .pem format (if using OpenSSH on Linux/macOS or Windows 10+ OpenSSH). Download the .pem file and keep it safe. Do NOT share it.
    6.	Network settings (Security group): Create a new security group with these inbound rules:
        o	SSH (TCP 22) — Source: your IP (for example, 203.0.113.45/32) or My IP (recommended). Do NOT open SSH to 0.0.0.0/0 (worldwide). To get your IP address, visit whatismyip.com. Copy your IPV4 address and paste.
        o	HTTP (TCP 80) — Source: 0.0.0.0/0 (if you plan to run a web server). Optional.
        o	HTTPS (TCP 443) — Source: 0.0.0.0/0 (optional).
    7.	Storage (EBS): Default 8–30 GiB is fine for practice. Ensure volume type is gp2 or gp3.
    8.	Advanced settings: Leave default for now. You can specify a user-data script to bootstrap the instance (optional).
    9.	Review and click Launch instance.
    10.	Confirm key pair selection, check the box acknowledging that you have access to the selected key pair, and click Launch Instances.
    11.	Click View Instances and wait for the instance state to become running and Status Checks to show 2/2 checks passed.
Important: Downloaded .pem key pair is only available at creation. If lost, you cannot download again (you must create a new key or use AWS Systems Manager to access the instance).


8. Connecting to your instance from Linux/macOS (SSH)
Assuming you have the .pem file on your Linux/macOS machine.
    1.	Move the .pem file to ~/.ssh and set strict permissions:
``` bash
        mkdir -p ~/.ssh
        mv ~/Downloads/ubuntu-ec2-key.pem ~/.ssh/
        chmod 600 ~/.ssh/ubuntu-ec2-key.pem
```
    2.	Find your instance's public DNS or IP in the EC2 Console (e.g., ec2-3-XX-XXX-XXX.compute-1.amazonaws.com or 3.XX.XXX.XXX).
    3.	Connect using the default Ubuntu user ubuntu:
``` bash
        ssh -i ~/.ssh/ubuntu-ec2-key.pem ubuntu@ec2-3-XX-XXX-XXX.compute-1.amazonaws.com
```
    4.	On first connect, accept the host key (yes). You should be logged into the Ubuntu shell.


 9. Connecting from Windows (PowerShell OpenSSH)
    Windows 10 and 11 include OpenSSH client. Steps:
        1.	Save your .pem file somewhere safe, e.g., C:\Users\YourUser\.ssh\ubuntu-ec2-key.pem.
        2.	Open PowerShell.
        3.	Set file permissions to be private (PowerShell):
``` bash
            icacls C:\Users\YourUser\.ssh\ubuntu-ec2-key.pem /inheritance:r
            icacls C:\Users\YourUser\.ssh\ubuntu-ec2-key.pem /grant:r YourUser:F
```
        4.	Connect:
            ssh -i C:\Users\YourUser\.ssh\ubuntu-ec2-key.pem ubuntu@ec2-3-XX-XXX-XXX.compute-1.amazonaws.com
        If connection is refused, ensure your security group allows SSH from your current public IP.


 10. Connecting from Windows using PuTTY (key conversion)
    PuTTY uses .ppk keys. Convert .pem to .ppk using PuTTYgen.
        1.	Download and open PuTTYgen.
        2.	Click Load, choose All Files, select your .pem file, and click Open.
        3.	Click Save private key and save as ubuntu-ec2-key.ppk.
        4.	Open PuTTY. In Session > Host Name, enter ubuntu@ec2-3-XX-XXX-XXX.compute-1.amazonaws.com.
        5.	Under Connection > SSH > Auth, browse for your .ppk file.
        6.	(Optional) Under Connection > Data, set Auto-login username to ubuntu.
        7.	Save the session and click Open. Accept host key and you will be connected.


 11. Using AWS EC2 Instance Connect and Session Manager (SSM)
EC2 Instance Connect (browser-based SSH):
•	Works for many Linux AMIs and requires the instance's security group to allow SSH from the service. In EC2 Console, select instance and click Connect > EC2 Instance Connect and follow prompts.
Session Manager (AWS Systems Manager SSM) — secure alternative to SSH (no open inbound ports):
1.	Attach the SSM IAM role to your instance (AmazonSSMManagedInstanceCore).
2.	Ensure SSM Agent is installed (Ubuntu 22.04 usually has it preinstalled; if not, install it).
3.	In AWS Console, go to Systems Manager > Session Manager and start a session to your instance.
Session Manager is great for learning because you avoid managing SSH keys and opening ports.


12. First steps on the server: updates, users, SSH hardening
Once connected (SSH or SSM), do these tasks.
1.	Update system packages:
``` bash
sudo apt update && sudo apt upgrade -y
```
2.	Create a regular user (if you want a separate user):
``` bash
sudo adduser devopslearner
sudo usermod -aG sudo devopslearner
```
3.	SSH hardening:
    Prefer key-based auth and disable password authentication. Edit /etc/ssh/sshd_config:
``` bash
    PasswordAuthentication no
    PermitRootLogin no
    PubkeyAuthentication yes
```
•	Restart sshd:
``` bash
sudo systemctl restart sshd
# or
sudo service ssh restart
```
4.	Add your public key to ~/.ssh/authorized_keys for your user.
5.	Configure UFW firewall:
``` bash
sudo apt install -y ufw
sudo ufw allow OpenSSH
sudo ufw enable
sudo ufw status
```

Assignment: Take out time to do a quick search on ssh and systemd in Linux. Share what you learn on Linkedln and mention Reuben Dickson


13. Installing common packages and setting up environment
Useful tools for learners:
``` bash
sudo apt install -y git htop build-essential curl wget tree unzip
```
The command above installs the following packages; git, build-essential, curl, wget, tree, unzip
# Install nginx as a simple web server
``` bash
sudo apt install -y nginx
sudo systemctl enable --now nginx
```
Check nginx in your browser using the instance public IP (port 80). If you used UFW, allow HTTP:
``` bash
sudo ufw allow 'Nginx HTTP'
```


14. Creating snapshots and AMIs (backups)
Create an EBS snapshot
1.	In EC2 Console, select Volumes, find the root volume, choose Create Snapshot.
2.	Add name and description. Create.
Create an AMI (image) of your instance
1.	Select your instance, Actions > Image > Create Image.
2.	Fill name and settings and create the AMI.
3.	You can later launch a new instance from this AMI with your configuration intact.


15. Cost management & how to avoid surprises
•	Use Free Tier instance types and stop them when not in use.
•	Terminate instances to avoid EBS storage costs; snapshots and AMIs also incur storage costs.
•	Tag resources with Project: learning-ubuntu and use cost explorer to filter.
•	Set Billing alerts in CloudWatch and enable the AWS Budgets service.


16. Stopping, starting, and terminating instances
•	Stop an instance to power it off but keep the EBS volume. Useful to pause charges on compute but EBS storage remains billed.
•	Start a stopped instance from EC2 Console.
•	Terminate deletes the instance and (usually) deletes the root EBS volume unless you changed settings. Use termination for cleanup when you no longer need the instance.


17. Troubleshooting common connection issues
    1.	Permission denied (publickey)
        o	Ensure you are using the correct username (ubuntu for Ubuntu AMIs).
        o	Ensure the private key file permissions are strict (chmod 600 on Linux/macOS).
        o	Confirm your security group allows SSH from your IP.
    2.	Connection timed out
        o	Security group doesn't allow inbound SSH.
        o	Network ACLs or corporate firewall blocking outbound SSH.
    3.	Instance state is stuck at pending
        o	AWS resource limits (unlikely for small accounts) or internal issues. Try launching in another Availability Zone or Region.
    4.	Cannot connect after changing sshd_config
        o	If you disabled password auth and removed your public key, you may lock yourself out. Use EC2 Instance Connect, Session Manager, or stop the instance and attach its root volume to a rescue instance to fix files.


18. Practice projects and exercises
    Project 1 — Simple web server
        •	Install nginx, put a custom index.html under /var/www/html, and view it via the instance public IP.
    Project 2 — Deploy a static website via Git
        •	Push a static site to GitHub, git clone on the instance, and serve with nginx.
    Project 3 — Create and restore snapshot
        •	Create an EBS snapshot, delete instance, and restore a new instance from AMI.
    Project 4 — Use Session Manager
        •	Attach SSM role and connect through the console using Systems Manager.


19. Command cheat sheet and appendix
# SSH from Linux/macOS
``` bash
    ssh -i ~/.ssh/ubuntu-ec2-key.pem ubuntu@ec2-3-XX-XXX-XXX.compute-1.amazonaws.com
```
# Move key and set permission
``` bash
mv ~/Downloads/ubuntu-ec2-key.pem ~/.ssh/
chmod 600 ~/.ssh/ubuntu-ec2-key.pem
```

# Update server
``` bash
    sudo apt update && sudo apt upgrade -y
```
# Add user and give sudo
``` bash
    sudo adduser devopslearner
    sudo usermod -aG sudo devopslearner
```

# UFW
``` bash
    sudo apt install -y ufw
    sudo ufw allow OpenSSH
    sudo ufw allow 'Nginx HTTP'
    sudo ufw enable

# Create snapshot (console recommended)
# Terminate instance
# (Use EC2 Console)