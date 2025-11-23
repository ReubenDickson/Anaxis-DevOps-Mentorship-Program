Real-World DevOps Automation Scenarios

Automation is one of the superpowers of a DevOps engineer. In real-world environments, you’ll often need to make sure systems keep running smoothly — even when you’re not watching. That’s where tools like cron, systemd timers, and bash scripts become invaluable.

Let’s explore a few practical DevOps use cases for scheduling and automation 👇

Scenario 1: Automatically Restarting a Failed Web Server

Problem:
Imagine you’re managing a production server running Node.js and Nginx. Sometimes, due to a memory leak or system overload, the Node.js process crashes unexpectedly.

Solution:
You can automate a check every 5 minutes to verify if the Node.js server is running. If not, it restarts it automatically.

Script Example: /home/student/monitor_node.sh
``` bash
#!/bin/bash

# Check if Node.js is running
if ! pgrep node > /dev/null
then
    echo "Node.js process not found! Restarting..." >> /var/log/monitor_node.log
    systemctl restart nodeapp.service
else
    echo "Node.js is running fine at $(date)" >> /var/log/monitor_node.log
fi
```

Cron Job (runs every 5 minutes):
``` bash
*/5 * * * * /home/student/monitor_node.sh
```

This simple script ensures your web application stays online — even when you’re asleep.



Scenario 2: Cleaning Up Old Log Files

Problem:
Servers generate logs daily. Over time, these logs consume disk space, potentially causing the server to slow down or even crash.

Solution:
Automate log cleanup every Sunday at midnight.

Script Example: /home/student/clean_logs.sh
``` bash
#!/bin/bash
find /var/log -type f -mtime +7 -name "*.log" -exec rm -f {} \;
echo "Old logs deleted successfully on $(date)" >> /var/log/cleanup.log
```

Cron Job:
``` bash
0 0 * * 0 /home/student/clean_logs.sh
```

This keeps your system clean and prevents disk overflow — a very common issue in real-world production systems.


Scenario 3: Automatic Database Backup and Sync to Remote Server

Problem:
A DevOps engineer must ensure that critical databases are backed up daily and safely stored on another machine or cloud.

Solution:
Use a cron job to back up the database and transfer it to a remote server with scp or rsync.

Script Example: /home/student/db_backup.sh
``` bash
#!/bin/bash

# Define variables
BACKUP_DIR="/tmp/db_backup"
REMOTE_USER="backupuser"
REMOTE_HOST="192.168.1.10"

# Create backup directory
mkdir -p $BACKUP_DIR

# Dump database
mysqldump -u root -p'yourpassword' mydatabase > $BACKUP_DIR/db-$(date +%F).sql

# Compress and send to remote server
tar -czf $BACKUP_DIR/db-$(date +%F).tar.gz -C $BACKUP_DIR .
scp $BACKUP_DIR/db-$(date +%F).tar.gz ${REMOTE_USER}@${REMOTE_HOST}:/backups/

# Log success
echo "Database backup completed on $(date)" >> /var/log/db_backup.log
```

Cron Job:
``` bash
30 1 * * * /home/student/db_backup.sh
```

This simulates how real companies protect their data — automatically, consistently, and securely.


Scenario 4: Daily Health Report

Problem:
As a system admin or DevOps engineer, you might want to receive a daily system report summarizing CPU, memory, and disk usage.

Solution:
Schedule a script that emails you or logs the system’s health.

Example Script: /home/student/system_report.sh
``` bash
#!/bin/bash

echo "System Health Report - $(date)" > /tmp/system_report.txt
echo "-----------------------------------" >> /tmp/system_report.txt
echo "CPU Usage:" >> /tmp/system_report.txt
top -bn1 | grep "Cpu(s)" >> /tmp/system_report.txt
echo "" >> /tmp/system_report.txt
echo "Memory Usage:" >> /tmp/system_report.txt
free -h >> /tmp/system_report.txt
echo "" >> /tmp/system_report.txt
echo "Disk Usage:" >> /tmp/system_report.txt
df -h >> /tmp/system_report.txt
```
# Optional: send via mail (if mailutils installed)
# mail -s "Daily System Report" admin@example.com < /tmp/system_report.txt


Cron Job (runs daily at 8 AM):
``` bash
0 8 * * * /home/student/system_report.sh
```

This helps you monitor your infrastructure proactively, before small issues become major outages.

Key Takeaways

Automation = Reliability: Systems that fix and maintain themselves reduce downtime.

Logging is crucial: Always log the actions your scripts perform.

Testing is vital: Always test scripts manually before scheduling them with cron.

Security first: Avoid saving plain-text passwords in scripts. Use environment variables or secret managers in production.