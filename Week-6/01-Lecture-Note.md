WEEK 6 — Linux Networking, Services & System Reliability
How Linux Servers Communicate, Serve Users, and Stay Online


Week 6 Purpose (Read This Carefully)
Until now, you have been working mostly inside the server.
In Week 6, we answer critical real-world questions:

i. How does a server communicate with the outside world?

ii. How do users reach applications?

iii. How do services start, stop, and recover?

iv. How do DevOps Engineers keep systems reliable?


This week prepares you mentally and technically for DevOps, Cloud, CI/CD, and Containers.


1. What Is Networking in Linux? (Big Picture)
Every server exists for one reason:

To communicate.

Servers:

- receive requests

- send responses

- talk to databases

- talk to other servers


If networking fails:

- users cannot access the app

- monitoring breaks

- deployments fail

You've definitely been doing networking all these while, you probably just didn't know.
When you as simply as connect your Android phone with another in order to share a file, you're networking.

Core Networking Concepts
Term                    Simple Meaning
IP Address              The server’s identity
Port                    A door on the server
Service                 A program listening on a port
Client                  A user or app making a request
Server                  A machine responding to requests

Example:
User → IP:PORT → Service → Response



2. Checking Network Configuration

View IP address
``` bash
ip addr show
```

Check routing
``` bash
ip route
```

Check DNS resolution
``` bash
ping google.com
```

If DNS fails, DevOps Engineers check:

- /etc/resolv.conf

- network connectivity




3. Ports & Listening Services

A service must listen on a port to be accessible.
View open ports
ss -tuln

Explaining the command:
-t (TCP): Filters the output to show only TCP (Transmission Control Protocol) sockets.
-u (UDP): Includes UDP (User Datagram Protocol) sockets in the output.
-l (Listening): Filters for sockets currently in the LISTEN state (those waiting for incoming connections).
-n (Numeric): Displays port numbers and IP addresses numerically (e.g., 80, 443, 22) instead of resolving them to service names like http, https, or ssh. 

Example output:
LISTEN 0 128 0.0.0.0:22
LISTEN 0 128 0.0.0.0:80

Interpretation:

Port 22 → SSH (you configured SSH in order to push your assignment to GitHub, remember?)

Port 80 → Web server (HTTP)



If a service isn’t listening, users cannot reach it.



4. Services & systemd (Core DevOps Skill)

Linux services are managed using systemd.

- Check service status
``` bash
systemctl status ssh
```

Start/Stop/Restart
``` bash
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
```

Enable service at boot (service starts automatically when system reboots)
``` bash
sudo systemctl enable nginx
```

Why DevOps Engineers Care

i. Services must restart after reboot

ii. Crashed services must recover

iii. Automation depends on services behaving predictably




5. Logs & Service Reliability
Logs tells us:

- why services failed

- why users were disconnected

- why deployments broke


View service logs
``` bash
journalctl -u nginx
```

Follow logs in real time
``` bash
journalctl -u nginx -f
```

Logs are evidence. DevOps Engineers don’t guess - they check logs.




6. Basic Service Monitoring & Self-Healing
DevOps Principle:

- Just assume things will fail — and design for recovery.

Create a simple monitoring script - /usr/local/bin/check_nginx.sh

``` bash

#!/bin/bash

if ! systemctl is-active --quiet nginx; then
  systemctl restart nginx
  echo "Nginx restarted at $(date)" >> /var/log/nginx-recovery.log
fi

Make executable:
chmod +x /usr/local/bin/check_nginx.sh

Schedule with cron:
*/5 * * * * /usr/local/bin/check_nginx.sh
```
This is basic self-healing infrastructure.