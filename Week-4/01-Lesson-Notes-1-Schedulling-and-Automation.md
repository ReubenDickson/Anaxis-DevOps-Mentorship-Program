Week 4 – Scheduling & Automation
Learning Objectives

By the end of this week, students should be able to:
Understand how automation works in Linux systems
Schedule recurring tasks using cron
Schedule one-time jobs using at
Understand and use systemd timers
Use file redirection and pipes to manage command outputs
Write and schedule simple bash scripts
Implement an automated daily backup task using cron



1. Introduction to Automation
Automation in Linux means making the system perform repetitive tasks automatically without human intervention.
As a DevOps Engineer, you’ll often need to automate:
- Backups
- Log rotation
- System updates
- Monitoring scripts
- Application restarts

Automation saves time, reduces human error, and ensures consistency in operations.



2. Understanding cron
Cron is a Linux utility used to schedule tasks (known as cron jobs) that run automatically at specific intervals — hourly, daily, weekly, etc.

How Cron Works
Cron runs in the background and checks the crontab (cron table) for jobs to execute.
Each user can have their own crontab file.
The system also has a global crontab file (/etc/crontab).

View Current Cron Jobs
``` bash
crontab -l
```

Edit or Create Cron Jobs
``` bash
crontab -e
```

Cron Time Format
A cron job follows this structure:
``` bash
* * * * * command_to_execute
```

Each * represents a time field:

Field	            Meaning         Valid Values
1	                Minute          0–59
2	                Hour	        0–23
3	                Day of Month	1–31
4	                Month	        1–12
5	                Day of Week	0–7 (0 and 7 = Sunday)

Example: Run Backup Script Every Day at 2 AM
``` bash
0 2 * * * /home/student/backup.sh
```


3. One-Time Scheduling with at

The at command schedules a task to run once at a specific time.

Example
``` bash
at 10:30
```

Then type:
``` bash
echo "System maintenance started" >> /home/student/maintenance.log
```

Press Ctrl + D to save.

Check Scheduled Jobs
``` bash
atq
```
Remove a Scheduled Job
``` bash
atrm <job_number>
```


4. Using systemd Timers

While cron is traditional, systemd timers offer more flexibility and better logging.

Example: Create a Timer

Create a service file:
``` bash
/etc/systemd/system/backup.service
```

[Unit]
Description=Daily backup of /etc directory

[Service]
ExecStart=/usr/local/bin/backup.sh


Create the timer file:
/etc/systemd/system/backup.timer

[Unit]
Description=Run backup.service daily

[Timer]
OnCalendar=daily
Persistent=true

[Install]
WantedBy=timers.target


Enable and start the timer:
``` bash
systemctl enable --now backup.timer
systemctl list-timers
```


5. File Redirection & Pipes

Linux allows you to redirect command outputs or chain commands using pipes.

File Redirection
Operator	Function	                                Example
>	        Redirect output to a file (overwrite)	    echo "Hello" > hello.txt
>>	        Append output to a file	                    echo "World" >> hello.txt
<	        Take input from a file	                    cat < file.txt
2>	        Redirect error output	                    ls /fake/dir 2> error.log

Pipes (|)
Used to send the output of one command as input to another.
``` bash
ps aux | grep nginx
```



6. Simple Bash Scripts

A bash script is a text file containing a series of Linux commands executed together.

Example Script: /home/student/backup.sh
``` bash
#!/bin/bash

# Create a backup directory if it doesn't exist
mkdir -p /tmp/backup

# Compress the /etc directory
tar -czf /tmp/backup/etc-$(date +%F).tar.gz /etc

# Print success message
echo "Backup completed successfully on $(date)"
```

Make the script executable:
``` bash
chmod +x /home/student/backup.sh
```


LAB: Automate Daily Backup of /etc to /tmp/backup with Cron
Step 1: Create the backup script
``` bash
nano /home/student/backup.sh
```

(Paste the script above)

Step 2: Make it executable
``` bash
chmod +x /home/student/backup.sh
```
Step 3: Schedule it with cron
``` bash
crontab -e
```

Add:
``` bash
0 2 * * * /home/student/backup.sh
```
Step 4: Verify cron is running
``` bash
systemctl status cron
```
Step 5: Check backup results

After 2 AM, verify the backup:
``` bash
ls /tmp/backup
```


Summary

In this lesson, you learned how to:

Automate tasks using cron, at, and systemd timers

Redirect and chain command outputs

Write and schedule bash scripts

Implement a daily system backup

Automation is one of the most powerful skills in Linux administration. Mastering it prepares you for more advanced DevOps tasks such as CI/CD pipelines and configuration management.