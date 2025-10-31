Real-World DevOps Scenario: Troubleshooting High CPU Usage in Production

Scenario Overview
You’re a Junior DevOps Engineer working for a fintech startup that runs an online payment application.
Users begin complaining that the site has become slow and sometimes unresponsive during peak hours.
Your task is to investigate the root cause of the slowdown using Linux monitoring tools (top, htop, ps, etc.) and recommend actions. But before we begin, let's get ourselves familiar with some key concepts.

Key Concepts Explained
Let’s take a moment to understand some of the important terms we used in our troubleshooting scenario.

I. Production (Environment)

In the world of software, “production” simply means the real environment where users access the application.

Think of it like this:

When developers are still building and testing an app, they do that in a test or development environment.

Once the app is ready and made available to customers — that’s the production environment.

Example:
When you use your bank’s mobile app to make a transfer — you are using the production version of that app.

The production environment must always be stable, secure, and fast, because it directly affects real users.

II. Node.js

Node.js is a technology (runtime) that allows developers to run JavaScript outside the browser — on a server.

Originally, JavaScript was meant only for websites (in browsers), but Node.js allows it to power backend servers and APIs.

Example:
A developer can use Node.js to create the backend of an app that handles login requests, payments, or fetching data from a database.

DevOps engineers often manage servers that run Node.js applications — making sure they stay online, perform well, and automatically restart if they crash.

III. Nginx (pronounced “Engine-X”)

Nginx is a web server — software that handles requests from users and sends back web pages or data.

When you type www.google.com, your browser sends a request to a server, and Nginx (if used there) helps process and deliver the page back to you.

Nginx is known for being:

Fast and lightweight

Capable of handling many users at once

Commonly used as a reverse proxy or load balancer (to distribute traffic between multiple servers)

DevOps engineers configure Nginx to ensure users can always reach an application — even when traffic is high.

IV. Server

A server is simply a powerful computer (often in the cloud) that stores, runs, and serves applications or websites to users.

It’s called a server because it serves requests from clients (like your phone or laptop).

Example:
When you visit www.amazon.com, your browser connects to Amazon’s servers, which then send you the website’s pages.

DevOps engineers manage servers — keeping them online, installing updates, monitoring performance, and ensuring security.

V. Database

A database is a place where data is stored, organized, and managed electronically.

It stores information in tables (like Excel sheets) so that applications can quickly save, retrieve, and update data.

Example:
When you log into your bank app, your account number, balance, and transaction history are all stored in a database.

Common databases include:

MySQL / MariaDB

PostgreSQL

MongoDB

DevOps engineers ensure databases are always available, backed up, and running efficiently — because if the database goes down, the app goes down.

In Summary:
Term	        |    Meaning	                                   |     Example
Production	    |    The real environment users interact with	   |     Your bank app
Node.js	        |    Runs backend JavaScript code	               |     Handles logins or API requests
Nginx	        |    Web server that delivers web pages	           |     Handles incoming traffic
Server	        |    The computer hosting your app	               |     AWS, Azure, or on-prem server
Database	    |    Stores application data	                   |         Stores user details, transactions


Now let's get back to investigating the root cause of our application slowing down.

The Environment
The application runs on an Ubuntu Server.
The backend consists of Node.js and Nginx.
The server has 2 CPUs and 4GB RAM.
You have SSH access to the server.

Step 1 – Connecting to the Server

You connect to the server via SSH:
``` bash
ssh ubuntu@10.0.2.15
```

Once connected, you begin your investigation.

Step 2 – Check System Load in Real Time

Start by viewing overall system usage:
``` bash
top
```

You notice the following:
``` bash
top - 10:21:44 up 2 days,  4:13,  2 users,  load average: 3.52, 2.97, 2.15
Tasks: 215 total, 1 running, 214 sleeping
%Cpu(s): 95.6 us,  3.1 sy,  0.0 ni,  1.3 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
Mem:   4048576 total, 3987652 used,   60924 free,   98760 buff/cache
```

Interpretation:

Load average: 3.52 means 3.5 processes waiting for CPU — too high for a 2-core system.

%Cpu(s): 95.6 us → user processes are consuming 95% CPU.

Memory almost full → might lead to swapping and slow performance.

You now suspect that a specific process is eating the CPU.

Step 3 – Identify CPU-Hungry Processes

Press P inside top to sort by CPU usage.
You notice one process stands out:
``` bash
PID  USER     PR  NI  VIRT  RES  SHR S  %CPU %MEM  TIME+ COMMAND
1423 ubuntu   20   0  885m  125m  25m R  90.5  3.2  3:15.23 node
```

So the Node.js process is consuming nearly 90% CPU continuously.

You note its PID: 1423.

Step 4 – Use htop for Better Visualization

Exit top (press q) and launch:
``` bash
htop
```

You see:

CPU bar nearly full (red zone)

Memory usage at 95%

One Node.js process (PID 1423) consuming ~90% CPU

A few Nginx workers around 2–3%

This visually confirms your suspicion.

Step 5 – Drill Deeper

You can use:
``` bash
ps -p 1423 -o pid,ppid,cmd,%cpu,%mem
```

Output:
``` bash
  PID  PPID  CMD                        %CPU %MEM
 1423  1201  node /var/www/app/server.js 90.5 3.2
```

You confirm this Node.js app (server.js) is the culprit.

Step 6 – Take Action

Before killing or restarting the process, you first check logs to understand what might be causing the issue.
``` bash
sudo journalctl -u nodeapp --since "1 hour ago"
```

You find repeated error lines:
``` bash
Error: Database connection timeout at db.connect()
```

This means the Node.js app keeps retrying to connect to the database, causing infinite loops and CPU spikes.

Root Cause Found: Misconfigured database connection in server.js.

Step 7 – Temporary Fix

Restart the Node.js service to reduce the immediate load:
``` bash
sudo systemctl restart nodeapp
```

Verify in htop that CPU usage drops:

%CPU now around 10–15%

Step 8 – Permanent Fix

Investigate application code: Check the database retry logic in server.js.

Improve connection handling: Add a timeout and retry limit.

Monitor performance regularly:

Use htop or top for live monitoring.

Use tools like Prometheus + Grafana (later in the DevOps phase).


DevOps Lesson Learned

As a DevOps Engineer:
You’re not just managing servers — you’re ensuring that applications run efficiently.

Monitoring tools (top, htop, journalctl) are your eyes into the system.

Always analyze before acting — killing processes blindly can cause downtime.

Communicate findings with the development team to prevent future issues.


Mini Lab: Practice

Try this on your local machine:

Open two terminals.

In one, run:
``` bash
yes > /dev/null
```

This will generate 100% CPU usage.

In the other, run:
``` bash
htop
```

Watch how your CPU spikes.

Find and kill the process using:
``` bash
killall yes
```

CPU returns to normal.

Summary
Command            |         Purpose
top                |         View real-time CPU/memory usage
htop               |         Visual process monitor
ps -p <PID>        |         Inspect specific process
kill, killall      |         Stop runaway processes
journalctl         |         Check service logs
systemctl restart  |         Restart system services