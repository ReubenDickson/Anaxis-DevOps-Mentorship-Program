WEEK 3 – Processes & System Monitoring
Lesson Objectives

By the end of this week, students should be able to:

Understand what a process is and how it runs in Linux

Monitor system activity and resource usage

Manage background and foreground jobs

View and interpret system logs

Write a bash script to list the top 5 CPU-consuming processes



1. Understanding Processes

A process is any running program or task in Linux.
Whenever you open an app or run a command, Linux starts a process for it.

For example:
``` bash
firefox &
```

This runs the Firefox browser as a process.

Each process has:

PID (Process ID): A unique number that identifies the process.

PPID (Parent Process ID): The process that started it.

STATE: Running, sleeping, or stopped.

USER: The user who owns the process.



2. Process Management Commands
- View Running Processes
``` bash
ps
```

This lists processes running in the current shell.

To see all processes:
``` bash
ps aux
```

Explanation:

a → all users

u → user-friendly format

x → include processes not attached to a terminal

To search for a specific process:
``` bash
ps aux | grep nginx
```

- Real-time Process Monitoring
Using top
``` bash
top
```

Shows a real-time view of CPU, memory, and running processes.

Using htop (more colorful and user-friendly)
Install it (if not available):
``` bash
sudo apt install htop
```

Then run:
``` bash
htop
```

Use F6 to sort by CPU or memory usage, and F9 to kill a process.

Killing a Process
Sometimes, a process hangs or consumes too many resources.

List processes:
``` bash
ps aux | grep processname
```

Then kill using its PID:
``` bash
sudo kill 1234
```

If it refuses to stop:
``` bash
sudo kill -9 1234
```
(Use with caution — it forcefully terminates the process.)



3. Background & Foreground Jobs

You can run tasks in the background so your terminal remains free.
- Run a command in the background
``` bash
sleep 100 &
```

The & tells Linux to run it in the background.

- Check running background jobs
``` bash
jobs
```
- Bring a job to the foreground
``` bash
fg %1
```

- Send a job back to the background
Press Ctrl + Z to suspend, then:
``` bash
bg %1
```


4. Logs & System Journal

Logs record system events, errors, and activities.
They are essential for troubleshooting and monitoring system health.

- View system logs
``` bash
cat /var/log/syslog
```

- View authentication logs
``` bash
cat /var/log/auth.log
```

- Using journalctl (modern log viewer)
``` bash
sudo journalctl
```

Show the most recent entries:
``` bash
sudo journalctl -r
```

Show logs for a specific service:
``` bash
sudo journalctl -u ssh
```

Follow logs in real time:
``` bash
sudo journalctl -f
```



5. LAB: Automating Process Monitoring
Task:

Let's write a Bash script that lists the top 5 CPU-consuming processes.

Script: top5cpu.sh
``` bash
#!/bin/bash
# A script to list the top 5 CPU-consuming processes

echo "Top 5 CPU-consuming processes:"
echo "-----------------------------------"
ps -eo pid,comm,%cpu,%mem --sort=-%cpu | head -n 6
```

Explanation:
ps -eo pid,comm,%cpu,%mem → show PID, command name, CPU, and memory usage

--sort=-%cpu → sort by CPU usage (descending)

head -n 6 → show top 5 + header row

Make your script executable, you have learnt how to do this.
Run the script:
``` bash
./top5cpu.sh
```

Output example:
``` bash
  PID COMMAND %CPU %MEM
 1234 firefox 35.5 2.1
 2211 chrome  22.3 1.8
 1456 code    15.9 3.0
  988 gnome   12.1 1.5
  501 bash     7.0 0.8
```



6. Practice Tasks

List all processes owned by your user.

Find the PID of the sshd process and kill it (don’t do this on remote SSH, else you'll get locked out).

Use htop to identify which process consumes the most memory.

Use journalctl -u nginx to check logs for your web server.

Modify the top5cpu.sh script to also display the top 5 memory consuming processes.



7. Summary
Command                                    Purpose
ps aux                                     List running processes
top / htop                                 Monitor system in real time
kill PID                                   Terminate a process
jobs, bg, fg                               Manage background jobs
journalctl                                 View system logs
ps -eo pid,comm,%cpu,%mem --sort=-%cpu     View top CPU-consuming processes