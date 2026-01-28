WEEK 5 — Filesystem & Storage
How Linux Stores Data, Why Disks Fill Up, and How DevOps Engineers Prevent Disasters

Week 5 Purpose (Read This First)

By Week 5, you are no longer just “learning Linux commands.”
You are learning how real systems behave over time.

Every production outage caused by:

- “Disk full”

- “No space left on device”

- “Logs grew uncontrollably”

- “Server stopped responding”

…is a filesystem and storage problem.


This week teaches you how DevOps Engineers:

- Understand how Linux stores data

- Detect storage problems early

- Control disk growth

- Protect systems using backups

- Design storage with the future in mind

If you stopped this program after Week 5, you would already understand one of the most common causes of real-world system failures.



1. How Linux Thinks About Storage

Linux storage follows a clear hierarchy:

``` bash
Physical Disk
   ↓
Partition
   ↓
Filesystem
   ↓
Mount Point (Directory)
   ↓
Files & Logs
```

Before touching commands, understand this clearly:

i. A disk is the physical storage (or cloud volume)

ii. A partition is a slice of that disk

iii. A filesystem defines how files are stored

iv. A mount point is where the filesystem appears in Linux

v. Everything becomes accessible through directories

In Linux, storage only exists if it is mounted.



2. Disks and Partitions - Seeing What Exists
Mental Model

Think of disks as empty warehouses.
Partitions are rooms inside them.
Filesystems are shelving systems.
Mount points are doors into those rooms.

View disks and partitions
``` bash
lsblk
```

Example output:
``` bash
NAME   SIZE TYPE MOUNTPOINT
sda     50G disk
├─sda1  48G part /
└─sda2   2G part [SWAP]
sdb    100G disk
```

Interpretation:

sda -> system disk

sda1 -> root filesystem /

sdb -> unused disk (dangerous if ignored)



3. Mounting & Unmounting — Making Storage Usable

Linux cannot use a disk unless it is mounted.

Temporary mount
``` bash
sudo mount /dev/sdb1 /mnt
```

Unmount
``` bash
sudo umount /mnt
```

Why this matters in DevOps

i. New disks are often added in cloud environments

ii. If not mounted, they are useless

iii. If mounted incorrectly, systems can fail to boot

A DevOps Engineer always verifies mounts.



4. Monitoring Disk Usage - Preventing “Disk Full”

Check overall disk usage
``` bash
df -h
```

Key question:

i. Which filesystem is filling up?

ii. How close is it to 100%?

Find what is consuming space
``` bash
du -sh /var/*
```

This answers:

“What exactly is eating my disk?”

In real life, the answer is almost always:

i. Logs

ii. Temporary files

iii. Application data



5. Logs: The Silent Disk Killers

Logs grow every second.

Linux stores logs mainly in:

/var/log


If unmanaged:

Disk fills up

Applications fail

System crashes

Services refuse to start

DevOps engineers never ignore logs.

🗜️ 6. Compression & Archiving — Controlling Growth
Create a compressed archive
tar -czvf logs.tar.gz /var/log


Explanation:

c → create

z → gzip compression

v → show progress

f → filename

Restore archive
tar -xzvf logs.tar.gz -C /tmp/restore


If you can’t restore a backup, it is not a backup.

🔁 7. rsync — Professional Backup Tool

Unlike cp, rsync:

Transfers only changes

Is fast and efficient

Works locally and remotely

rsync -av /var/log/ /tmp/log_backup/


This is industry-grade backup behavior.

🔥 REAL-WORLD DEVOPS PROJECT (WEEK 5 CORE)
🏗️ Project: Production Log Management & Storage Protection
🧩 Scenario

You manage a production Ubuntu server for a fintech company.
Over time, users report slow performance.

Investigation shows:

/var/log is growing uncontrollably

Root filesystem is at 92%

Risk of downtime is high

Your task is to design and implement a storage-safe log system.

🎯 Project Objectives

You must:

Analyze disk usage

Mount additional storage for archives

Compress logs daily

Retain logs for a fixed period

Delete old archives automatically

Prove logs can be restored

🛠️ Implementation (Step-by-Step)
Step 1 — Analyze
df -h
du -sh /var/log

Step 2 — Prepare archive storage
sudo mkdir -p /mnt/log_archive


(Assume disk already mounted or simulate directory)

Step 3 — Backup script

/usr/local/bin/archive_logs.sh

#!/bin/bash

ARCHIVE_DIR="/mnt/log_archive"
DATE=$(date +%F)

mkdir -p $ARCHIVE_DIR
tar -czf $ARCHIVE_DIR/logs-$DATE.tar.gz /var/log

Step 4 — Retention policy
find /mnt/log_archive -type f -mtime +14 -delete

Step 5 — Restore test
mkdir /tmp/log_restore
tar -xzvf /mnt/log_archive/logs-YYYY-MM-DD.tar.gz -C /tmp/log_restore

🧠 HOW A DEVOPS ENGINEER THINKS (WEEK 5 CORE)

Storage grows silently — monitor it

Logs are valuable but dangerous

Backups without restore tests are useless

Automation prevents emergencies

Prevention is cheaper than recovery

📁 STUDENT DELIVERABLES

Students must submit:

week5-filesystem-project/
├── scripts/
│   └── archive_logs.sh
├── documentation/
│   ├── disk_analysis.md
│   ├── backup_strategy.md
│   └── restore_procedure.md
└── README.md


With:

Screenshots of disk usage

Archive files created

Restore verification

Clear explanations

✅ WHAT THIS WEEK GIVES STUDENTS

Even if a student stops here, they now understand:

How Linux stores data

Why disks fail

How logs destroy systems

How backups protect uptime

How DevOps engineers think about storage

This is real foundational DevOps knowledge.