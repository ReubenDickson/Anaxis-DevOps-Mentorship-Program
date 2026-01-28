WEEK 6 REAL-WORLD PROJECT
“Deploy, Monitor, and Protect a Web Service”

Scenario: You are responsible for a Linux server hosting a company website.
Requirements:

i. Website must be reachable

ii. Service must start on boot

iii. Failures must be detected

iv. Downtime must be minimized

v. Logs must be available



Project Tasks

i. Install a web server
``` bash
sudo apt install nginx -y
```

ii. Verify access
``` bash
curl localhost
```

iii. Enable service at boot
``` bash
sudo systemctl enable nginx
```

iv. Create monitoring script - Use check_nginx.sh above.

v. Test failure recovery
``` bash
sudo systemctl stop nginx
```

Wait 5 minutes → confirm auto-restart.

vi. Document logs
``` bash
journalctl -u nginx > nginx_logs.txt
```