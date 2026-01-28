REAL-WORLD DEVOPS PROJECT (WEEK 5 CORE)
Project: Production Log Management & Storage Protection

Scenario: You manage a production Ubuntu server for a fintech company.
Over time, users report slow performance.

Investigation shows:

- /var/log is growing uncontrollably

- Root filesystem is at 92%

- Risk of downtime is high

Your task is to design and implement a storage-safe log system.


Project Objectives

You must:

i. Analyze disk usage

ii. Mount additional storage for archives

iii. Compress logs daily

iv. Retain logs for a fixed period

v. Delete old archives automatically

vi. Prove logs can be restored


Implementation (Step-by-Step)

Step 1 — Analyze
``` bash
df -h
du -sh /var/log
```

Step 2 — Prepare archive storage
``` bash
sudo mkdir -p /mnt/log_archive
```

(Assume disk already mounted or simulate directory)

Step 3 — Backup script

``` bash
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
```
