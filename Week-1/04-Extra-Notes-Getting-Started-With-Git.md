 
6-Month DevOps Engineering Mentorship Program (Week-by-Week)

Prep: Getting Started with GitHub — A Practical Beginner's Guide
Understanding Git and Github is essential for DevOps. This is a step-by-step practical guide. The objective is to give learners a working knowledge of Git and GitHub, and by the end of this guide, you will have created, pushed, and published a simple website (HTML/CSS/JavaScript) to GitHub.

What you'll learn
•	What Git and GitHub are, and why they matter for DevOps
•	How to create a GitHub account and set up secure access (HTTPS and SSH)
•	Git basics: init, add, commit, branch, merge, push, pull
•	Using GitHub: repositories, README, issues, Pull Requests (PRs), branches
•	Tools: Git CLI, GitHub Desktop, and VS Code integration
•	Create a simple website project and push it to GitHub
•	Deploy the static site using GitHub Pages
•	Best practices, troubleshooting, and exercises for learners


1. Why Git and GitHub? — Plain language
•	Git is a version control system. It keeps track of changes to files over time so you can go back, compare, or work with others without losing work.
•	GitHub is an online service that hosts Git repositories. It allows collaboration, code review, issue tracking, and (for our case) hosting static websites via GitHub Pages.
For DevOps learners, Git/GitHub are essential: infrastructure as code, CI/CD, collaboration — all use Git.

What Is Version Control?
Version control (also known as source control) is a system that helps you track changes to files, store different versions of those files, and collaborate with others on a project — all without losing previous work.
It’s like having a time machine for your code or documents. You can go back to see who made what change, when, and why.

Why Version Control Is Important
Imagine you’re building a website with a team:
•	You write the HTML code.
•	Your teammate adds some CSS styling.
•	Another person improves the JavaScript.
Without version control, this can quickly become messy — people might overwrite each other’s changes or lose important updates. With version control, every change is recorded, organized, and reversible.
Here’s what version control allows you to do:
1.	Track every change – See exactly what changed and when.
2.	Collaborate easily – Multiple people can work on the same project simultaneously.
3.	Recover old versions – If something breaks, you can roll back to a previous stable version.
4.	Experiment safely – You can create new “branches” to test ideas without affecting the main project.
5.	Document your progress – Each update can include a “commit message” describing what was done.

Two Types of Version Control Systems
1. Centralized Version Control (CVCS)
All files are stored in a central server, and users get a copy from that server.
•	Example: Subversion (SVN)
•	Pros: Easier to control access, single source of truth.
•	Cons: If the main server fails, everyone loses access.
2. Distributed Version Control (DVCS)
Every developer has a full copy of the repository (including history) on their local machine.
•	Example: Git
•	Pros: Work offline, full history available, faster operations, safer backups.
•	Cons: Slightly more complex for beginners, but far more powerful.

How Git (the Most Popular Version Control System) Works
Git stores your project in a repository (repo) and tracks changes using commits.
Here’s the basic workflow:
1.	Initialize a new repository
``` bash
git init
```
2.	Add files to track
``` bash
git add index.html
```
3.	Commit the changes
``` bash
git commit -m "Added homepage"
```
4.	View history
``` bash
git log
```
5.	Create a new branch for new features
``` bash
git branch new-feature
```
6.	Switch between branches
``` bash
git checkout new-feature
```
7.	Merge changes
``` bash
git merge new-feature
```
8.	Push to remote repository (like GitHub)
``` bash
git push origin main
```

How Version Control Helps in DevOps
For DevOps engineers, version control is a foundation of the entire workflow. It helps you:
•	Manage code for infrastructure automation (e.g., Terraform, Ansible).
•	Maintain consistency between development, staging, and production environments.
•	Work effectively with CI/CD pipelines (Continuous Integration and Deployment).
•	Collaborate across teams (developers, sysadmins, testers).


2. Prerequisites
•	A laptop or desktop (Windows, macOS, or Linux).
•	Internet connection.
•	A free GitHub account (we will create one).
•	Basic willingness to copy-and-paste commands. We'll explain everything.
Optional tools (recommended):
•	Visual Studio Code (VS Code) — lightweight code editor with Git integration.
•	GitHub Desktop — GUI for Git operations (for learners who prefer not to use command line yet).


3. Create a GitHub account (step-by-step)
    1.	Open your browser and go to https://github.com/.
    2.	Click Sign up. Enter an email address, create a password, and choose a username (unique). Example: anaxis-dev.
    3.	Follow the on-screen verification steps (email confirmation, CAPTCHA).
    4.	Choose the free plan when asked.
    5.	Optionally add profile details later.
Security tip: Enable Two-factor authentication (2FA) in Settings > Security after you finish setting up. This protects your account.


4. Install Git locally on Ubuntu (Using Terminal)
``` bash
sudo apt update
sudo apt install -y git
# Verify installation
git --version
```


5. Configure Git (global)
``` bash
git config --global user.name "Your Name"
git config --global user.email “youremail@example.com”
Check configuration:
git config --list
```
Optional — set default branch name to main
``` bash
git config --global init.defaultBranch main
```


6. GitHub access methods: HTTPS vs SSH
HTTPS (easy): You use your GitHub username/password or a Personal Access Token (PAT) when pushing. PAT is recommended because GitHub removed password-based authentication for Git operations.
SSH (secure & convenient): You create an SSH key pair and add the public key to GitHub. After setup, you can push/pull without entering credentials.
We'll show both methods. Beginners can start with HTTPS and later add SSH.


7. Create SSH key and add to GitHub (recommended)
Generate SSH key (Linux/macOS/Windows Git Bash):
``` bash
cd ~/.ssh
ssh-keygen -t ed25519 -C "you@example.com”
# Give your new key a name, eg "github-access-key"
```

Accept default file location (~/.ssh/id_ed25519) and choose a passphrase (recommended). Then add key to ssh-agent:
``` bash
eval "$(ssh-agent -s)"
# Make your private key readable by you alone
chmod 600 github-access-key
ssh-add github-access-key # replace with the name of your 
# Confirm that your key is loaded
ssh-add -l
```
Copy public key to clipboard
Linux/macOS:
``` bash
cat github-access-key
# Copy the output
```

Add key on GitHub
1.	On GitHub, go to Settings > SSH and GPG keys > New SSH key.
2.	Paste the public key and give it a title (e.g., Laptop - Anaxis).
3.	Save.

Test SSH connection
``` bash
ssh -T git@github.com
```
You should see a welcome message.


8. Create your first repository on GitHub (web)
    1.	Sign in to GitHub.
    2.	Click + (top-right) > New repository.
    3.	Repository name: devops-portfolio.
    4.	Description: My DevOps Portfolio Website.
    5.	Choose Public.
    6.	Check Add a README file.
    7.	(Optional) Add .gitignore > choose Node or None (for this simple project, none required).
    8.	Click Create repository.
You now have an empty repository with a README. We shall return to this later.


9. Initialize local Git repository and push to GitHub (CLI)
Option A — Clone a GitHub repo and make changes to the files.
Go to https://github.com/ReubenDickson/dev-ops-portfolio
Click on the '<> Code' dropdown menu.
Use the HTTPS or SSH clone URL.
Using SSH (if set up):

Create a new folder named devops-portfolio on your machine and cd the the folder.

``` bash
mkdir devops-portfolio
cd devops-portfolio
```

clone Reuben Dickson's devops-portfolio repository.
``` bash
git clone https://github.com/ReubenDickson/dev-ops-portfolio .
```
Do not ignore the dot at the end of the previous command, it tells the computer to clone the repo into the current directory. You should have the following files in your folder; index.html, README.md, and an ‘images’ folder.
Using HTTPS (if not using SSH):
``` bash
git clone https://github.com/ReubenDickson/dev-ops-portfolio .
```

If you alread have Visual Studio Code installed, launch it using the command below, or open it using your prefered text editor.
``` bash
code .
```

Now add your photo in the images folder. Edit ‘index.html’ and update the link to point to your photo in the images folder. Find this on Line 156.
If you have copied your image into the 'images' folder, rename it to 'profile'. Also note your image format whether it's jpg or png. Now edit that line to look like this;
``` bash
        <img src="images/profile.jpg" alt="Your name">
```
You might also want to update the Linkedln link on line 175 as well as your email address.
Now go back to your Terminal and make sure you are in the same folder.

``` bash
git add .
git commit -m "Initial commit: my devops-portfolio website"
# Update git remote origin to now point to your repo instead of mine.
git remote set-url origin git@github.com:your-username/devops-portfolio.git
git branch -M main
git push -u origin main -f
```
If GitHub prompts for credentials for HTTPS, use your GitHub username and a Personal Access Token (PAT) as the password. You can create a PAT in GitHub Settings > Developer settings > Personal access tokens. For repo pushes, repo scope is needed.


11. View repository on GitHub and add README details
    1.	On GitHub repo page, you'll now see uploaded files.
    2.	Click README.md to edit and add a short guide about the project, how to run it locally, and credits.
Example README content:
DevOps portfolio Website

A beginner project for getting familiar with Github for DevOps.

 Run locally
1. Open `index.html` in your browser.
Commit README changes directly on GitHub or edit locally and push.


12. Deploy the site with GitHub Pages (publish)
GitHub Pages can host static sites from the main branch or gh-pages branch.
Simple approach (main branch)
    1.	Go to your repository on GitHub.
    2.	Settings > Pages (on the left menu).
    3.	Under "Build and deployment", choose Deploy from a branch.
    4.	Branch: main, folder: / (root) and click Save.
    5.	After a minute, GitHub will provide a URL like https://yourusername.github.io/devops-portfolio/.
Open that URL to see your site live.


13. Basic Git workflows for beginners
•	Make changes locally → git add → git commit → git push
•	To get updates from others: git pull
•	To create a feature branch and open a Pull Request (PR):
``` bash
git checkout -b feature/change-text
# make edits
git add .
git commit -m "Change welcome message"
git push -u origin feature/change-text
```
On GitHub, click Compare & pull request for your branch, add description, then Create pull request. Use PRs for code review and collaboration.


14. A few best practices for learners
•	Commit often with clear messages (git commit -m "Fix typo in index.html").
•	Use .gitignore to avoid committing OS files or secrets. Example .gitignore:
node_modules/
.DS_Store
*.env
•	Never commit secrets (API keys, passwords). Use environment variables or GitHub Secrets (for actions).
•	Use branches for features and PRs for merging.


15. Troubleshooting common issues
Permission denied (publickey)
•	You likely cloned using SSH but haven't added your SSH key to GitHub. Add the key or clone via HTTPS.
Authentication failed for HTTPS
•	Use a Personal Access Token (PAT) instead of password for HTTPS pushes.
Push rejected — non-fast-forward
•	Someone updated the remote. Run git pull --rebase then git push.
Files changed unexpectedly (line endings)
•	Windows uses CRLF, Linux uses LF. Configure Git to handle this:
``` bash
git config --global core.autocrlf true  # Windows
git config --global core.autocrlf input # macOS/Linux
```

16. Command cheat sheet (quick reference)
 Git identity
``` bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```


Initialize repo
``` bash
git init
```


Cloning
``` bash
git clone git@github.com:yourusername/repository-name.git
```

Basic workflow
``` bash
git status
git add .
git commit -m "Message"
git push origin main

# Branching
git checkout -b feature/xxx
git push -u origin feature/xxx

# Pull changes
git pull

# Show history
git log --oneline --graph --decorate
```


19. Additional resources
•	Official Git docs: https://git-scm.com/doc
•	GitHub Learning Lab: https://lab.github.com/
•	Interactive Git tutorial: https://learngitbranching.js.org/
•	VS Code Git docs: https://code.visualstudio.com/docs/editor/versioncontrol