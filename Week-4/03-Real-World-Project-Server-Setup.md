WEEK 4 REAL-WORLD PROJECT — “Server Setup, User Management, Monitoring & Automation for a Small Business”
🔎 Project Name:

Small Business Linux Server Setup & Automation Project

🎯 Goal:

Students will simulate setting up and managing a new Linux server for a small business — handling users, security, logs, processes, tasks, and automation.

This project gives them something real and practical to showcase on their GitHub.

🧩 1. PROJECT BACKGROUND

A small company called GreenFarm Analytics purchases a new Linux server (Ubuntu) to manage their business operations.

They hire YOU as the junior Linux/DevOps Engineer to help with the following:

Configure the server for multiple users

Set permissions for different roles

Monitor system performance

Automate daily tasks

Secure and organize logs

Create helpful scripts to simplify operations

This is exactly what junior DevOps engineers do in real internships/jobs.

🛠️ 2. WHAT YOU MUST IMPLEMENT
✔️ 2.1. Basic Server Setup (Week 1 Skills)

Using Linux commands:

Create a folder structure that looks like this:

/company_data
/company_data/admin
/company_data/analytics
/company_data/interns


Create a welcome message script located in /usr/local/bin/welcome.sh

Display system info when run:

echo "Welcome, today is $(date)"
echo "System uptime:"
uptime
echo "Current users logged in:"
who


Add execute permissions for all users.

✔️ 2.2. Users & Permissions Setup (Week 2 Skills)

GreenFarm has the following employees:

Username	Role	Access Needed
alice	Admin	Full access to all folders
bob	Analyst	Access to analytics folder
charlie	Intern	Read-only access to interns folder

You must:

Create users: alice, bob, charlie

Create groups: admins, analysts, interns

Add each user to the right group

Assign permissions:

/company_data/admin → full access for admins

/company_data/analytics → full access for analysts

/company_data/interns → read-only for interns

Set folder permissions using:

chown

chmod

group access (chgrp)

✔️ 2.3. Process Monitoring & Troubleshooting (Week 3 Skills)

Create a monitoring script:

/usr/local/bin/monitor_processes.sh

#!/bin/bash
echo "Top 5 CPU Processes:"
ps -eo pid,comm,%cpu,%mem --sort=-%cpu | head -n 6


Make it executable.

Run and save the output to /company_data/admin/system_report.txt using redirection.
Example:

/usr/local/bin/monitor_processes.sh > /company_data/admin/system_report.txt

✔️ 2.4. Scheduling Daily Tasks (Week 4 Skills)

You must automate:

1️⃣ A daily clean-up task

Create script:
/usr/local/bin/cleanup.sh

#!/bin/bash
find /company_data/interns -type f -mtime +3 -delete
echo "Cleaned intern files older than 3 days on $(date)" >> /var/log/cleanup.log


Schedule it with cron:

0 3 * * * /usr/local/bin/cleanup.sh

2️⃣ A daily report task

Automatically generate a system report:

/usr/local/bin/daily_report.sh

#!/bin/bash
echo "System Report - $(date)" > /company_data/admin/daily_report.txt
df -h >> /company_data/admin/daily_report.txt
free -h >> /company_data/admin/daily_report.txt


Cron:

0 6 * * * /usr/local/bin/daily_report.sh

✔️ 2.5. A Menu-Based Automation Tool (BONUS)

Create a script that gives users a simple menu:

/usr/local/bin/admin_menu.sh

#!/bin/bash
echo "1. View system uptime"
echo "2. View top 5 cpu processes"
echo "3. Backup company data"
read -p "Choose an option: " choice

case $choice in
    1) uptime;;
    2) /usr/local/bin/monitor_processes.sh;;
    3) tar -czf /tmp/company_backup.tar.gz /company_data;;
esac

🎓 3. WHAT STUDENTS MUST SUBMIT
📁 GitHub Structure
week1-4-project/
├── scripts/
│   ├── welcome.sh
│   ├── monitor_processes.sh
│   ├── cleanup.sh
│   ├── daily_report.sh
│   └── admin_menu.sh
│
├── documentation/
│   ├── user_setup.md
│   ├── permissions_structure.md
│   ├── cron_jobs.md
│   ├── monitoring_report.txt
│   └── troubleshooting_steps.md
│
└── README.md

📸 Screenshots to include:

Folder structure

Users & groups created

Permissions listing (ls -l)

Running scripts

Cron jobs

Output of reports

🌟 4. What Students Will Learn (Massive Value)

By completing this project, students master:

🔹 Week 1

Linux navigation, directories, command line mastery, file editing

🔹 Week 2

Users, groups, permissions, ownership, security

🔹 Week 3

Process monitoring, troubleshooting, logs, system observation

🔹 Week 4

Automation, cron jobs, bash scripting, redirection & pipes

This becomes their first complete Linux + DevOps portfolio project.

This project is perfect for:

GitHub portfolio

Job interviews

Internship applications

LinkedIn posts

Demonstrating foundational DevOps competency