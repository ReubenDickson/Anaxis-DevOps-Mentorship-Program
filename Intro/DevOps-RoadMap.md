 
6-Month Junior DevOps Engineer Mentorship Program (Week-by-Week)

Phase 1: Linux Foundation (Weeks 1–12)
Goal: Build strong Linux/Ubuntu administration and Git foundation.

Month 1 – Linux Basics


Week 1 – Getting Started with Linux
•	Topics:
o	Linux architecture, kernel, shell
o	Navigating file system (ls, cd, pwd)
o	Files & directories (cp, mv, rm, mkdir)
o	Editors: nano, vim
•	Lab:
o	Create folders/files, edit with vim, move/copy them.
o	Write a short note on Linux vs Windows differences.


Week 2 – Users & Permissions
•	Topics:
o	Users/groups (adduser, passwd, usermod)
o	Permissions (chmod, chown, umask)
o	Sudo & privilege escalation
•	Lab:
o	Create 3 users, assign groups/permissions.
o	Restrict one user from accessing a directory.


Week 3 – Processes & System Monitoring
•	Topics:
o	Process management (ps, top, htop, kill)
o	Background jobs (bg, fg, jobs)
o	Logs & journal (journalctl)
•	Lab:
o	Write a script to list top 5 CPU-consuming processes.


Week 4 – Scheduling & Automation
•	Topics:
o	cron, at, timers
o	File redirection & pipes
o	Simple bash scripts
•	Lab:
o	Automate daily backup of /etc to /tmp/backup with cron.



Month 2 – Advanced Linux & Networking


Week 5 – Filesystem & Storage
•	Topics:
o	Partitions, mount/unmount
o	df, du, lsblk
o	Compression & archiving (tar, gzip, rsync)
•	Lab:
o	Compress /var/log and restore it.


Week 6 – Networking Basics
•	Topics:
o	IP addressing, ping, curl, wget
o	ss, netstat, traceroute
o	SSH & SCP
•	Lab:
o	Connect to another student’s VM via SSH & transfer files.


Week 7 – Services & Systemd
•	Topics:
o	systemctl (start/stop/enable services)
o	Writing systemd unit files
o	Logs & troubleshooting services
•	Lab:
o	Deploy a simple Nginx server as a service.


Week 8 – Shell Scripting Deep Dive
•	Topics:
o	Variables, conditions, loops
o	Functions & arguments in scripts
•	Lab:
o	Script to check disk usage and send warning if >80%.



Month 3 – Security, Troubleshooting & Git


Week 9 – Linux Security
•	Topics:
o	ufw, firewall basics
o	SSH hardening (keys, disabling root login)
o	File integrity checks
•	Lab:
o	Secure SSH with key authentication only.


Week 10 – Troubleshooting
•	Topics:
o	Debugging service failures
o	Checking system logs
o	Finding bottlenecks with top, iotop, iftop
•	Lab:
o	Break a service intentionally and troubleshoot it.


Week 11 – Version Control (Git Basics)
•	Topics:
o	Git basics: init, clone, commit, push, pull
o	Branching & merging
o	GitHub repo creation & collaboration
•	Lab:
o	Students upload all their Linux scripts to GitHub.


Week 12 – Git Advanced & Mini Project
•	Topics:
o	Pull requests, issues, GitHub workflow
•	Lab/Project:
o	Create a Git repo for automation scripts.
o	Submit changes via pull request.




Phase 2: DevOps Foundation (Weeks 13–24)
Goal: Introduce DevOps tools & workflows—automation, containers, CI/CD, cloud, Kubernetes.


Month 4 – DevOps & Automation


Week 13 – Introduction to DevOps
•	Topics:
o	DevOps culture, CI/CD basics
o	Agile & Kanban overview
•	Lab:
o	Create a Kanban board in GitHub Projects for tracking tasks.


Week 14 – Infrastructure as Code (Ansible Basics)
•	Topics:
o	Install Ansible
o	Writing simple playbooks (install packages, configure services)
•	Lab:
o	Automate Nginx setup with Ansible.


Week 15 – Containerization with Docker
•	Topics:
o	Docker basics (images, containers, volumes, networks)
o	DockerHub
•	Lab:
o	Build & run a Docker container with a simple Python app.


Week 16 – Docker Advanced & Compose
•	Topics:
o	Dockerfile best practices
o	Docker Compose for multi-container apps
•	Lab:
o	Deploy WordPress + MySQL with Docker Compose.


Month 5 – CI/CD & Cloud

Week 17 – CI/CD with GitHub Actions
•	Topics:
o	Writing workflows for build/test/deploy
•	Lab:
o	Automate testing & build of Docker app with GitHub Actions.


Week 18 – Cloud Fundamentals (AWS)
•	Topics:
o	EC2, S3, IAM basics
o	Deploying Docker app on EC2
•	Lab:
o	Deploy containerized app to AWS EC2.

Week 19 – Monitoring & Logging
•	Topics:
o	Prometheus & Grafana basics
o	Log management with ELK stack
•	Lab:
o	Monitor server CPU/memory with Prometheus + Grafana.

Week 20 – Cloud Automation with Ansible
•	Topics:
o	Provision EC2 instances with Ansible
•	Lab:
o	Write playbook to install Docker on EC2.


Month 6 – Kubernetes & Final Project

Week 21 – Kubernetes Basics
•	Topics:
o	Pods, Deployments, Services
o	Minikube/Kind setup
•	Lab:
o	Deploy containerized app on Kubernetes.


Week 22 – Kubernetes Advanced
•	Topics:
o	ConfigMaps & Secrets
o	Scaling & rolling updates
•	Lab:
o	Add scaling policy to deployed app.


Week 23 – DevOps Final Project (Part 1)
•	Project Setup:
o	Containerize app → Push to DockerHub
o	GitHub Actions → CI pipeline
o	Deploy to Kubernetes


Week 24 – DevOps Final Project (Part 2) & Career Prep
•	Project Completion:
o	Add monitoring with Prometheus + Grafana
o	Documentation & GitHub repo cleanup
•	Career Prep:
o	Resume building
o	Mock interviews (Linux, Docker, Kubernetes, Git)



Final Outcome

By Week 24, you will:
•	Have GitHub repos of Linux scripts, Ansible playbooks, Dockerfiles, and Kubernetes manifests
•	Have deployed apps to cloud (AWS) and Kubernetes
•	Understand CI/CD pipelines with GitHub Actions
•	Be ready to interview as Junior DevOps Engineers