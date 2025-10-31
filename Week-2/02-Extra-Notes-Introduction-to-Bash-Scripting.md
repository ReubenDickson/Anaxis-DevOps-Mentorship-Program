Introduction to Bash Scripting
(Automating User and Group Management)

1. What is Bash Scripting?

So far, we’ve been typing Linux commands one after another in the terminal — for example:
``` bash
sudo adduser student1
sudo usermod -aG developers student1
sudo passwd student1
```

This is fine for small tasks, but what if you needed to create 10 users, set their passwords, and add them to groups?
Doing that manually would take forever 😩 — and that’s where Bash scripting comes in!

Bash scripting lets you:

- Automate repetitive tasks

- Combine multiple commands into a single file

- Run those commands anytime, anywhere, with just one click (or one line)

- Think of it as giving Linux a “to-do list” — and it executes it exactly as written.



2. Creating Your First Bash Script

Let’s write our first Bash script.

Open your terminal and create a new file:
``` bash
nano create_users.sh
```

Inside the file, type the following:
``` bash
#!/bin/bash
# A simple script to create a user and group automatically

echo "Enter username:"
read username

echo "Enter group name:"
read groupname

# Create group if it doesn’t exist
if ! getent group $groupname >/dev/null; then
    sudo groupadd $groupname
    echo "Group '$groupname' created successfully."
else
    echo "Group '$groupname' already exists."
fi

# Create the user and add them to the group
sudo useradd -m -G $groupname $username
echo "User '$username' added to group '$groupname'."

# Set a password
echo "Set password for $username:"
sudo passwd $username

echo "User and group setup completed successfully!"
```

Save the file (CTRL + O, then Enter, then CTRL + X).

Line-by-Line Explanation

Line 1: #!/bin/bash
What it is: This is called a "shebang" or "hashbang"
What it does: It tells the computer: "Hey, use the bash program to understand and run this script!"
Just like writing "Instructions in English" at the top of a document

Line 2: # A simple script to create a user and group automatically
What it is: A comment (starts with #)
What it does: It's just a note for humans reading the script - the computer ignores it

Line 4: echo "Enter username:"
What it does: Displays the text "Enter username:" on the screen

Line 5: read username
What it does: Waits for you to type something and saves what you type into a "box" (a variable) called username

Lines 7-8: Same as above but for group name
echo "Enter group name:"
read groupname
What it does: Asks for and saves the group name

Line 11: if ! getent group $groupname >/dev/null; then
if: Means "check if something is true"
getent group $groupname: Looks in the system's "group phonebook" to see if this group already exists
!: Means "NOT" (so we're checking if the group does NOT exist)
>/dev/null: Silences any output (hides technical messages)
then: Means "if the check is true, do the following lines"

Lines 12-13: Creating the group
sudo groupadd $groupname
echo "Group '$groupname' created successfully."
sudo: Means "do this with administrator power" (like using a master key)
groupadd: The command that actually creates the group
echo: Print a message on the screen, in this case telling you it worked

Line 14: else
What it does: Means "if the group already exists, do this instead"

Line 15: echo "Group '$groupname' already exists."
What it does: Tells you the group was already there

Line 18: sudo useradd -m -G $groupname $username
useradd: Command to create a new user
$username: The username you typed earlier

Line 19: echo "User '$username' added to group '$groupname'."
What it does: Confirms the user was created and added to the group

Line 23: sudo passwd $username
passwd: Command to set or change a password
This will make the computer ask you to type and confirm the new password

Line 25: echo "User and group setup completed successfully!"
What it does: Final confirmation that everything is done



3. Making the Script Executable

Before running your script, you must give it execution permission:
``` bash
sudo chmod +x create_users.sh
```

Now run the script:
``` bash
./create_users.sh
```

It will ask you to enter a username and group name, then automatically create them and set the password!



4. Adding Multiple Users Automatically

Here’s a slightly advanced example. Let’s say you have a text file with usernames, one per line:

users.txt

john
mary
paul
chioma


You can create a script that loops through each line:

``` bash
#!/bin/bash
# Create multiple users from a list

for user in $(cat users.txt)
do
    sudo useradd -m $user
    echo "User $user created successfully."
done
```

When you run this:
``` bash
./create_multiple_users.sh
```

…it will automatically create every user in the file!



5. Why This Matters in DevOps

Bash scripting is the foundation of automation — a key skill in DevOps.
As a DevOps Engineer, you’ll automate:

Server setup

User management

Backups

Deployments

Monitoring and updates

Every command you’ve learned can be scripted and reused — that’s how professionals manage hundreds of servers at once.



6. Practice Task
Write a bash script named manage_users.sh that:

Creates a group called devops_team

Adds three users to it (user1, user2, user3)

Sets passwords for each

Prints a confirmation message after each step



7. Summary

Bash scripting automates Linux tasks.

Scripts are just text files with commands.

Always start with #!/bin/bash.

Use chmod +x to make scripts executable.

Practice = mastery



8. Week challenge (mandatory — upload to GitHub)

Write user_setup.sh — helps automate onboarding. The script must:

Accept a username as argument (or prompt if no argument).

Create the user and a group with the same name.

Create the home directory if not created.

Add the user to sudo group.

Create /home/<username>/welcome.txt with a short welcome message.

Print status messages as it runs and exit with code 0 on success.

Example (starter script — students must improve and handle errors):

``` bash
#!/bin/bash

# simple user_setup.sh starter
if [ -z "$1" ]; then
  read -p "Enter username to create: " USERNAME
else
  USERNAME="$1"
fi

# check running as root
if [ "$(id -u)" -ne 0 ]; then
  echo "Please run as root or with sudo"
  exit 1
fi

# create group and user, create home
groupadd -f "$USERNAME"
useradd -m -s /bin/bash -g "$USERNAME" "$USERNAME" || { echo "Failed to create user"; exit 2; }

# set password (optional): uncomment to force
# echo "${USERNAME}:ChangeMe123!" | chpasswd

# add to sudo
usermod -aG sudo "$USERNAME"

# create welcome file
echo "Welcome $USERNAME to the DevOps Mentorship Program!" > /home/"$USERNAME"/welcome.txt
chown "$USERNAME":"$USERNAME" /home/"$USERNAME"/welcome.txt
chmod 644 /home/"$USERNAME"/welcome.txt

echo "User setup completed successfully!"
exit 0
```

Submission: Push to GitHub repo linux_basics/week2/user_setup.sh with a README showing how you tested (screenshots or terminal logs).