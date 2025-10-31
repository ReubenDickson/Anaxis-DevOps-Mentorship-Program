Users & Permissions

Goal (by week’s end):
You should be able to create/manage users and groups, understand file ownership, set and interpret permissions (symbolic and numeric), use sudo, and apply basic user-security best practices.


1. Why Users & Permissions matter (in plain language)

Linux is a multi-user system: many people (or services) can use the same machine.

Permissions prevent accidental or malicious access: they control who can read, change, or run files.

Proper user/group setup keeps production systems secure and organized.

You’ll use these skills daily as a DevOps engineer (deploying services, configuring servers, securing keys, etc.).

Analogy: think of a shared office filing cabinet. Each drawer (file) has labels (owner/group) and locks (permissions). You decide who gets a key (read), who can add/remove papers (write), and who can open it (execute).



2. User accounts — what they are and where information is stored

Important files:

/etc/passwd — lists users (human-readable info, no passwords).

/etc/shadow — stores hashed passwords (restricted access).

/etc/group — lists groups and group members.

Quick view:

- List user names
``` bash
cut -d: -f1 /etc/passwd | less
```

- Show groups
``` bash
cat /etc/group | less
```

- Never open /etc/shadow as normal user (permission restricted)
``` bash
ls -l /etc/shadow
```

- A line from /etc/passwd looks like this:
``` bash
anaxis:x:1001:1001:Anaxis User:/home/anaxis:/bin/bash
```
Fields (colon-separated): username, placeholder for password (x), UID (user ID), GID (group ID), comment (full name), home directory, shell.



3. Creating users and groups (step-by-step)

Use 'sudo' for commands that change system configuration.

Create a user interactively (recommended)
``` bash
sudo adduser student1
```

- What adduser does:

i. creates /home/student1

ii. sets ownership and default files (.bashrc)

iii. prompts for password and optional info (you can press Enter to skip)

Create a user without prompts
``` bash
sudo useradd -m -s /bin/bash -G devops student2
sudo passwd student2
```

-m create home directory

-s /bin/bash set default shell

-G devops add to secondary group 'devops' (group must exist)

Create a group
``` bash
sudo groupadd devops
```

Add an existing user to a group
``` bash
sudo usermod -aG devops student1
# verify
groups student1
```

-aG = append the user to supplementary groups (don’t forget -a or you’ll overwrite groups).

Remove a user
``` bash
sudo deluser --remove-home student2
```

--remove-home deletes the home directory too — use with caution.



4. File ownership and the ls -l output (breakdown)

Run:
``` bash
ls -l /home
```

Example line:

drwxr-x--- 3 anaxis devops 4096 Oct 24 12:00 myproject


Fields:

d — type (d=directory, -=file, l=symlink)

rwxr-x--- — permissions (owner | group | others)

3 — hard link count

anaxis — owner user

devops — owner group

4096 — size (bytes)

date/time — last modification

myproject — file/directory name

Permissions split:

Owner (first 3 chars): r w x - owner can read, write and execute

Group (next 3): r - x - group can read and execute but not write

Others (last 3): - - -  - other have no permission



5. Changing ownership: chown and chgrp

Change owner:
``` bash
sudo chown alice myscript.sh      # change owner to alice, group unchanged for the file 'myscript.sh'
sudo chown alice:devops mydir     # change owner to alice and group to devops for the folder 'mydir'
```

Change group only:
``` bash
sudo chgrp devops mydir
```

Recursive change (for directories):
``` bash
sudo chown -R alice:devops /var/www/myapp
```

-R applies to all files/subdirs inside.



6. Changing permissions: chmod (symbolic and numeric) — explained carefully
Symbolic mode (human readable)

Format: chmod [who][op][perms] file

who: u (user/owner), g (group), o (others), a (all)

op: + add, - remove, = set exactly

perms: r read, w write, x execute

Examples:
``` bash
chmod u+x run.sh      # add execute for owner
chmod g-w secret.txt  # remove write from group
chmod o=r notes.txt   # set others to read only
chmod a-x script.sh   # remove execute for everyone
```

Numeric (octal) mode — step-by-step digit math (do this carefully)

Each permission bit has a value:

r = 4

w = 2

x = 1

For each of owner, group, others, sum bits to form a digit (0–7).

Example: owner rwx = 4+2+1 = 7

group r-x = 4+0+1 = 5

others r-- = 4+0+0 = 4

So rwx r-x r-- → 754: # This means the owner has read, write and execute permissions, group have read and execute, while others have only read permissions.

``` bash
chmod 754 myscript.sh
```

Always compute digit by digit. If you want 644 (common for files):

owner rw- = 4+2 = 6

group r-- = 4

others r-- = 4

Set it:

chmod 644 document.txt

Do you recall running the command below in our previous lesson:
``` bash
chmod 600 ~/.ssh/ubuntu-ec2-key.pem
```
Can you now tell what it does? (Smiles)

Special bits (overview)

Sticky bit (directory): restricts deletion inside directory to owner of file or owner of directory (t in others exec place). Common on /tmp.

Setuid / setgid: advanced; run an executable with owner’s permissions — use cautiously. We'll introduce when needed.



7. Sudo and privilege escalation

sudo allows permitted users to run commands as root. Typical flow:
``` bash
sudo apt update
sudo systemctl restart nginx
```

Add user to sudo group:
``` bash
sudo usermod -aG sudo alice    # Debian/Ubuntu
```

Verify:
``` bash
sudo whoami
# expected: root
```

Edit /etc/sudoers with caution (always use visudo):
``` bash
sudo visudo
```

visudo checks syntax before saving — avoids locking you out.

Best practice: Avoid enabling passwordless sudo unless absolutely necessary.



8. Common real-world tasks & examples
Create a shared project folder for team
- Create group and users
``` bash
sudo groupadd projectx
sudo useradd -m -s /bin/bash -G projectx rita
sudo useradd -m -s /bin/bash -G projectx chike
```

- Create folder and assign group ownership
``` bash
sudo mkdir /srv/projectx
sudo chown :projectx /srv/projectx
sudo chmod 2770 /srv/projectx
```

Explanation:

chmod 2770 sets owner rwx, group rwx, others ---, and the 2 sets the setgid bit so new files inherit the group projectx.

Restrict logs to owner and root only
``` bash
sudo chown root:adm /var/log/securelog
sudo chmod 640 /var/log/securelog
```

Owner root can read/write, group adm can read, others none.



9. Lab exercises (do these one by one — practice is mandatory)

Environment options: VirtualBox VM, WSL, cloud Ubuntu instance (AWS/GCP/Azure free tier), or an online linux playground. For file ownership and sudo tests, use a real Ubuntu VM or cloud instance (WSL has different behavior for some permission bits).

Lab 1 — Create users & groups

Create two users: alice, bob with adduser.

Create group devteam.

Add both users to devteam.

Verify with groups alice and getent group devteam.

Lab 2 — Shared project folder

Create /home/shared_proj.

Set owner root and group devteam.

Ensure group members can create files and files they create automatically belong to group devteam. (Use chmod 2770 above.)

From alice’s account, create a file alice_note.txt. From bob, try to edit it (should have group permissions).

Commands (example):
``` bash
sudo mkdir /home/shared_proj
sudo chown root:devteam /home/shared_proj
sudo chmod 2770 /home/shared_proj
```

- As alice
``` bash
su - alice
cd /home/shared_proj
touch alice_note.txt
ls -l
```

Lab 3 — Permission modes practice

Create /home/testperms/file1.txt and set 644.

Set file2.sh to be executable only by owner (700).

Create public.txt readable by everyone (644).

Use ls -l to confirm.


Lab 4 — Sudo & user creation script

Create the week challenge script (see next section). Run and fix issues until it works.




10. Troubleshooting & debugging tips

If usermod fails with “user not known”: check /etc/passwd.

If permissions don’t behave as expected: check ACLs with getfacl (advanced) and sticky/setgid bits.

If you can’t sudo: ensure your user is in sudo group and relogin (newgrp sudo or logout/login).

For file ownership oddities on shared mounts: verify mount options (e.g., vboxsf, CIFS behave differently).

Useful commands:

id alice
getent passwd alice
getent group devteam
getfacl /path/to/file    # advanced
stat myfile              # detailed file metadata



11. Security best practices (beginner checklist)

Don’t use root for daily tasks. Use sudo.

Remove unused users and disable inactive accounts.

Use SSH keys for server access, not passwords (we’ll cover in later weeks).

Use groups to manage permissions instead of giving broad owner access.

Regularly audit /etc/sudoers and sudo group membership.



12. Further reading & commands cheat-sheet

Cheat commands:

adduser <name> — friendly interactive user creation

useradd -m -s /bin/bash <name> — scriptable user creation

groupadd <group> — create group

usermod -aG <group> <user> — add to group

deluser --remove-home <user> — delete user and home

chown user:group file — change owner/group

chmod 755 file — set permissions numeric

ls -l — list with permissions

stat file — extended info

Recommended docs:

man adduser, man useradd, man chown, man chmod

Online: Ubuntu Server Guide (search “Ubuntu user management”)



13. Homework (must submit)

Complete Labs 1–4. Add screenshots or terminal logs.

Write and test user_setup.sh. Push to GitHub with README explaining tests.

Post a LinkedIn update describing what you learned this week (compulsory). Include 2–3 commands you used and one screenshot.