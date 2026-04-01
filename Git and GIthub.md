# Git and Github

## What is Git?
“Git is a version control system used to track changes in code and enable team collaboration.”

### What it does
•	tracks code changes  
•	maintains history  
•	allows multiple people to work together  


## Question: What is version control?

Version control is a system that helps us track changes in code over time. It allows multiple developers to work on the same project, maintain history, and revert to previous versions if needed.

________________________________________


## Question: What is the difference between Git and version control system?

Version control system is a general concept used to track code changes, while Git is a specific tool that implements version control. Git is a distributed version control system.

________________________________________


## Question: What is the difference between Git and GitHub?

Git is a version control tool used to track changes locally on a system, while GitHub is a cloud platform used to store Git repositories and collaborate with others.  
Extra point:  
Git works offline, but GitHub requires internet and is used for sharing code.


## What is a repository in Git?
A repository is a storage location where all the project files, code, and their history are kept. It tracks all changes made to the project.

________________________________________


## What is a local repository?
A local repository is stored on your own system. It is where you make changes, commit code, and manage versions locally.

________________________________________


## What is a remote repository?
A remote repository is stored on a server or cloud platform like GitHub. It is used to share code and collaborate with others.


## What is a working directory in Git?
It is the place where we write and modify our code. It contains the current files of the project.

________________________________________


## What is the staging area in Git?
The staging area is where we add files before committing. It acts like a checkpoint where we decide what changes to include in the next commit.

________________________________________


## What is a commit in Git?
A commit is a saved snapshot of the project. It stores the changes along with a message for tracking history.

________________________________________


## What is a branch in Git?
A branch is a separate line of development. It allows us to work on features or changes without affecting the main code.

________________________________________


## Why do we use branches in Git?
We use branches to work on new features, fix bugs, or test changes without disturbing the main code. It helps in safe development and collaboration.

________________________________________


## What is the default branch in Git?
The default branch is usually main. It is the primary branch where the stable and final code is maintained.


## What is HEAD in Git?
HEAD is a pointer that tells which branch or commit you are currently working on. It usually points to the latest commit of the current branch.

________________________________________


## What is the difference between tracked and untracked files?
Tracked files are those that Git is already monitoring and managing.  
Untracked files are new files that Git is not tracking yet until we add them using git add.

________________________________________


## ✅ Complete Git File Lifecycle

### 🔹 Case 1: New file (first time)

Step-by-step:

1.	Create a new file  
👉 File is UNTRACKED  

2.	Add file to Git  
git add file.txt  
👉 Now file is:  
•	TRACKED  
•	STAGED  

3.	Commit the file  
git commit -m "added file"  
👉 Now file is:  
•	TRACKED  
•	COMMITTED (saved in local repo)  


### 🔹 Case 2: Already tracked file (modified)

Step-by-step:

1.	Modify the file  
👉 File becomes:  
•	MODIFIED (not staged yet)  

2.	Add changes  
git add file.txt  
👉 Now file is:  
•	STAGED  

3.	Commit changes  
git commit -m "updated file"  
👉 Changes are saved in local repo  


### 🎯 Final Simple Understanding (MOST IMPORTANT)

🟢 First time file:  
Untracked → git add → Staged + Tracked → git commit  

🔵 After that:  
Modify → Modified → git add → Staged → git commit  



## How do you initialize a Git repository?
We use:  
git init  
This creates a new Git repository in the current folder.

________________________________________


## How do you clone a repository?
We use:  
git clone <repository-url>  
This copies an existing remote repository to the local system.

________________________________________


## How do you check the status of files in Git?
We use:  
git status  
It shows modified, staged, and untracked files.

________________________________________


## How do you add files to staging?
To add all files:  
git add .  

To add a specific file:  
git add filename  

This moves changes to the staging area.

________________________________________


## What is the difference between git add and git commit?
git add moves changes to the staging area.  
git commit saves those staged changes in the local repository.

## What exactly happens in add and commit

git add  
When you do git add, Git takes the current version of the file and puts it in staging area.  
It means:  
“I want this version to go into the next commit.”  

git commit  
When you do git commit, Git takes the staged content and saves it as a snapshot in the local repository.  

Very important  
git commit saves locally only.  
It does not go to GitHub yet.  

For GitHub, you must do:  
git push  

________________________________________


## How do you commit changes?
We use:  
git commit -m "your message"  
This saves the staged changes with a commit message.

________________________________________


## How do you view commit history and revert back to previous commit ?
We use:  
git log  
It shows the commit history of the repository.  

Git reset –hard <commit-id> //by doing this the head will point to the particular commit-id

________________________________________


## How do you push code to remote repository?
We use:  
git push origin main  
sends the commits from your local repository to the remote repository on the main branch.

________________________________________


## What if remote is ahead
Suppose GitHub main branch has new commits, but your local main does not.  
Then if you try to push, Git may reject it.  

So first do:  
git pull origin main  

Then push again:  
git push origin main  


## What is a merge conflict?
A merge conflict happens when Git is unable to automatically merge changes because the same part of a file was changed in different ways in two branches or in local and remote code.  
We solve it by opening the conflicted file, editing it manually, removing conflict markers, then adding and committing the resolved file  

Example:  

Your branch:  
name = "Shashank"  

Other branch:  
name = "DevOps"  

Git does not know which one to keep.  

So it shows conflict markers like:  

<<<<<<< HEAD  
name = "Shashank"  
=======  
name = "DevOps"  
>>>>>>> main  

You must edit manually, keep the correct code, remove markers, then:  

git add file.txt  
git commit -m "resolve conflict"  


________________________________________


## What is the difference between git pull and git fetch?

git fetch  
Downloads latest changes from remote into local Git internal storage.  
It does not update your current working files.  

git pull  
Downloads latest changes and merges them into your current branch.  

Simple memory trick  
•	fetch = download only  
•	pull = download + merge  


Where fetch downloads changes  

When you do:  
git fetch  

Git stores remote updates in local Git data inside .git folder, as remote-tracking branches like origin/main.  
Your actual project files do not change immediately.

________________________________________


## Branching

Branches are used so that we can work safely without disturbing main code.  

Main idea  

Suppose main branch has stable code.  
You want to add login feature.  

Instead of changing main directly, you create a new branch:  

git branch login-feature  

Then switch to it:  

git switch login-feature  

Now your changes happen in this branch only.  

Later, when work is complete, merge it into main.  


Why branches are used  

•	feature development  
•	bug fixing  
•	safe testing  
•	team collaboration  


Create branch  
git branch new-branch  

Switch branch  
git switch new-branch  

Create and switch together  
git switch -c new-branch  

Merge branch  

First go to main:  
git switch main  

Then merge:  
git merge new-branch  

Delete branch  
git branch -d new-branch  

To rename the current branch:  
git branch -m new-branch-name  


Why is branching important?  
Branching is important because it allows safe development. We can work on features, bug fixes, or experiments without breaking the main (stable) code. It also helps multiple developers work in parallel.


## What is merging in Git?
Merging means combining changes from one branch into another branch, usually from a feature branch into the main branch.


## Branching strategy followed in development

### 🔹 1. Main / Master branch

👉 What it is:  
This is your live production code  

👉 When used:  
•	Only stable, tested code goes here  
•	This is what users see  

👉 Example:  
Your website is running → code in main  

________________________________________

### 🔹 2. Feature branch

👉 What it is:  
Used to build new features  

👉 When used:  
Whenever you start new work  

👉 Example:  
You want to add:  
👉 Login feature  

git switch -c login-feature  

You work here safely without touching main  

________________________________________

Flow:  
main → feature → main  

________________________________________

### 🔹 3. Release branch

👉 What it is:  
Used when your code is ready for release but needs testing  

👉 When used:  
•	Features are done  
•	Now you want final testing  

________________________________________

Example:  
You completed:  
•	login feature  
•	dashboard  

Now:  
git switch -c release-1.0  

👉 In this branch:  
•	Only testing  
•	Only bug fixes  
❌ No new features  

________________________________________

Flow:  
develop → release → main  

________________________________________

### 🔹 4. Hotfix branch

👉 What it is:  
Used to fix urgent production issues  

👉 When used:  
•	Bug in live app  
•	Cannot wait  

________________________________________

Example:  

Website is live  
Login is broken ❌  

git switch -c hotfix-login main  

👉 Fix quickly  
👉 Push to main  

Flow:  
main → hotfix → main  

Note:Any branch we created at the end will merge with main branch  


## git cherry-pick

git cherry-pick is used when we want to take one specific commit from one branch and apply it to another branch using the commit ID.  

Eg Say for example we have second branch named feature which is ahead of main branch my one commit  

We can see that by using command git log feature and git log main  

Say we want that one commit which is ahead in feature brach to be added in main branch  

So we give command git cherry-pick <commit -id>  


## Git merge ,here we will not get linear commit history

Suppose this is the history:  

A --- B --- C   (main)  
          \  
           D --- E   (feature)  

Here:  
•	main has 3 commits → A, B, C  
•	feature has 2 extra commits → D, E  

Now if you switch to main and run:  

git merge feature  

What happens?  

Git combines both branch histories and creates a new merge commit.  

Result:  

A --- B --- C -------- M   (main)  
          \          /  
           D --- E   (feature)  

Meaning  
•	M is the merge commit  
•	main now contains:  
o	old commits A, B, C  
o	feature commits D, E  


## git rebase , we will get the liner commit history

🧠 After rebase  

A --- B --- C --- D' --- E'   (feature)  

👉 D' and E' are new commits (rewritten)  
👉 No merge commit  
👉 History is linear  


________________________________________


## What is the difference between git checkout and git switch?
git checkout is older and can do multiple things like switching branches and restoring files.  
git switch is newer and mainly used only for switching branches, so it is simpler and clearer.

________________________________________


## How do you view differences between files?

git diff  

Simple meaning  
It compares:  

•	current file in your folder  
with  
•	staged version of that file  

So it shows:  
“What changes did I make after the last git add  

git diff –staged  

Simple meaning  
It compares:  

•	staged version of the file  
with  
•	last committed version of the file  

So it shows:  
“What changes are staged and will go into the next commit?”  


git diff file1 file2 → Compares file1 vs file2 content directly  


________________________________________


## Steps from creating a Git repo to pushing code to GitHub

First, we create a folder for the project and keep our files inside it.  

git init   -> This converts the normal folder into a Git repository.  
git status   -> This shows which files are new, modified, or not yet tracked.  
git add .  -> This moves the files to the staging area  
git commit -m "Initial commit"   ->This saves the files in the local repository with a message  
git remote add origin <repo-url>    -> This links the local repository to the remote GitHub repository.  
git push -u origin main -> This sends the local committed code to the GitHub repository.  


## What is the difference between Git and SVN?

Git is a distributed version control system where every developer has a full copy of the repository.  
SVN is a centralized version control system where all data is stored on a central server.  

### Key differences (simple points to add if asked):

•	Architecture:  
Git → Distributed  
SVN → Centralized  

•	Working:  
Git → Local commits + push later  
SVN → Direct commit to central server  

•	Speed:  
Git → Faster (local operations)  
SVN → Slower (depends on server)  

•	Branching:  
Git → Easy and fast  
SVN → Complex and slower  

•	Offline work:  
Git → Possible  
SVN → Not possible  


## Centralized Version Control (like SVN)

•	There is one central server that stores all code  
•	Developers connect to that server to work  
•	Need internet/server access to commit changes  
•	If server is down → work is affected  
•	No full backup on local system  

________________________________________


## Distributed Version Control (like Git)

•	Every developer has a full copy of the repository  
•	Can work offline and commit locally  
•	Changes are shared later using push/pull  
•	Faster because most operations are local  
•	More secure since multiple copies exist  


## .git folder

.git folder is the internal storage of Git where all repository data like commits, branches, and configuration are stored.  

It consists of  

•  HEAD → points to the current branch  
•  index → stores staged files  
•  objects → stores actual data like commits and file content  
•  refs → stores branch and tag pointers  
•  config → contains repository settings  
•  hooks → used to run scripts during Git events  


git log --oneline //show git logs in one line  


## What is Git Flow?

Git Flow is a branching strategy where we use multiple branches for different purposes:  

•	main → production code  
•	develop → ongoing development  
•	feature → new features  
•	release → final testing  
•	hotfix → urgent fixes  


## What is trunk-based development?
“Trunk-based development is a strategy where developers frequently merge small changes into a single main branch instead of maintaining multiple long-lived branches.”


## What is a public repository?
A public repository is a repository that anyone on the internet can view and access.

________________________________________


## What is a private repository?
A private repository is a repository that is accessible only to the owner and selected collaborators.

________________________________________


## What is a fork in GitHub?
A fork is a copy of someone else’s repository into your own GitHub account, allowing you to make changes independently.

________________________________________


## What is a clone in GitHub?
A clone is a copy of a GitHub repository downloaded to your local system so you can work on it locally.


## What is a pull request?
A pull request is a request to merge changes from one branch into another branch (usually into main) in GitHub.

________________________________________


## What is the purpose of a pull request?
The purpose is to:  

•	review code  
•	discuss changes  
•	ensure quality before merging  

________________________________________


## What is code review in GitHub?
Code review is the process where team members check the code changes in a pull request before approving and merging them.

________________________________________


## Why is code review important?
It helps to:  

•	catch bugs early  
•	improve code quality  
•	maintain coding standards  
•	share knowledge among team members  

________________________________________


## What is a merge request (MR)?
A merge request is the same concept as a pull request but used in platforms like GitLab. It is a request to merge changes from one branch to another.

________________________________________


## What is the difference between pull request and merge request?
There is no functional difference.  

•	Pull Request → term used in GitHub  
•	Merge Request → term used in GitLab  


## What is GitHub Actions?
GitHub Actions is a tool in GitHub used to automate workflows like building, testing, and deploying code.

________________________________________


## What is the use of GitHub Actions in DevOps?
It is used to automate CI/CD processes such as:  

•	running tests automatically  
•	building applications  
•	deploying code to servers  

________________________________________


## What is a webhook in GitHub?
A webhook is a way for GitHub to send real-time notifications to another system when an event happens (like push or pull request).

________________________________________


## What is the use of webhooks in CI/CD?
Webhooks are used to trigger automation.  

For example:  
•	when code is pushed → trigger build  
•	when PR is created → trigger testing  

________________________________________


## What is a GitHub issue?
A GitHub issue is used to track tasks, bugs, or feature requests in a project.


## 🔥 Visual understanding

Original repo (company/project) → upstream  

Your fork (you/project) → origin = your GitHub repository  

Your laptop → local repo  


Upstream is used to refer to the original repository in a forked setup, and it is mainly used to pull the latest updates into our local repository.  


## Why do we need upstream?

Because original repo keeps changing:  

•	new features added  
•	bugs fixed  
•	updates released  

👉 You need those updates also  

________________________________________


## 🔹 Use case flow

### Step 1: Get latest updates from original repo

git pull upstream main  

👉 Now your local repo is updated  

________________________________________


### Step 2: Push updated code to your repo

git push origin main  

👉 Now your GitHub fork is also updated  


## 🔥 Full real workflow

upstream (original repo)  
        ↓ pull  
local repo  
        ↓ push  
origin (your repo)  


## How do you connect a local repo to a remote repo?
git remote add origin <repo_url>  

## What happens when you push code?
syncs local changes to remote repo  

## What happens when you pull code?
So it updates my local code with remote changes.  

## What is a remote tracking branch?
helps Git track what is happening in the remote without modifying my local branch directly.  

## How do you list remote repositories?
git remote -v  

## How do you remove a remote repository?
git remote remove origin  

## Add a new remote repository  or Link local repo to platforms like GitHub or GitLab
git remote add origin <repo_url>  

________________________________________


## 1. What is a protected branch in GitHub?
A protected branch is a branch where direct changes are restricted to keep the code safe and stable.  
Usually, it prevents direct push, requires pull requests, code reviews, and sometimes passing CI checks before merge.  

________________________________________


## 2. How do multiple developers work on the same repository?
Multiple developers work on the same repository by using separate branches.  
Each developer creates their own branch, works on their changes, and then raises a pull request to merge into the main branch.  
This helps avoid direct changes in the main code and makes collaboration easier and safer.  

________________________________________


## 3. How does Git help in DevOps?
Git helps in DevOps by providing version control and enabling collaboration, traceability, and automation.  
It keeps track of every code change, who made it, and when it was made.  
It also integrates with CI/CD tools, so code changes can automatically trigger build, test, and deployment pipelines.  

________________________________________


## 4. Why is Git important for CI/CD pipelines?
Git is important for CI/CD because the pipeline usually starts when code is pushed to the repository.  
For example, when a developer pushes code or merges a pull request, Git can trigger automated build, test, and deployment steps.  

________________________________________


## 5. How do you handle conflicts in a team environment?

If a conflict happens, I first understand which change should be kept, discuss with the team if needed, then resolve it and test the code before committing.  


## Authentication and access

### 1. How do you authenticate with GitHub?
We authenticate with GitHub using:  
•	HTTPS with Personal Access Token (PAT)  
•	SSH keys  

Instead of username/password, we now use secure methods like PAT or SSH for authentication.  

________________________________________


### 2. What is SSH key in Git?
An SSH key is a secure key pair used for authentication:  
•	Public key → added to GitHub  
•	Private key → stays on my system  

It allows me to connect to GitHub without entering credentials every time.  

________________________________________


### 3. What is the difference between HTTPS and SSH in Git?
•	HTTPS → uses username + PAT for authentication  
•	SSH → uses key-based authentication (no need to enter credentials repeatedly)  

HTTPS is easier to set up, but SSH is more secure and convenient for regular use.  

________________________________________


### 4. Why is SSH preferred?
SSH is preferred because:  
•	No need to enter credentials every time  
•	More secure (key-based authentication)  
•	Better for automation and frequent Git operations  

________________________________________


### 5. What is Personal Access Token (PAT)?
A Personal Access Token is a secure token used instead of a password for GitHub authentication over HTTPS.  
It has specific permissions like repo access and improves security.  

________________________________________


### 6. Why are passwords not used anymore for GitHub push?
Passwords are removed because they are less secure and can be easily compromised.  
GitHub replaced them with PAT and SSH keys to provide better security, access control, and safer authentication.  

## 1) If you made a wrong commit, how would you fix it?

### Case 1: Last commit is wrong and not pushed

Command  
git reset --soft HEAD~1  

What it does  
It removes the last commit but keeps the changes in staging.  

Example  
git add app.py  
git commit -m "wrong commit message"  
git reset --soft HEAD~1  

Now the commit is removed, but the changes are still there.  
Then make the correct commit:  
git commit -m "correct commit message"  

________________________________________

### Case 2: Commit is already pushed

Command  
git revert <commit_id>  

Example  
git log --oneline  
git revert a1b2c3d  
git push origin main  

What it does  
It creates a new commit that reverses the wrong commit.  
This is safer in team environments.  

________________________________________

## 2) If you accidentally deleted a branch, how can you recover it?

Step 1: Check reflog  
git reflog  

Step 2: Find the commit id of that branch  

Example output:  
abc1234 HEAD@{3}: commit: added login feature  

Step 3: Recreate the branch  
git checkout -b feature-login abc1234  

Example  
git reflog  
git checkout -b feature-login abc1234  

This recreates the deleted branch from that commit.  

________________________________________

## 3) If your code is outdated compared to remote, what will you do?

Step 1: Pull latest code  
git pull origin main  

Example  
git checkout main  
git pull origin main  

If you are on your feature branch  
First update main:  
git checkout main  
git pull origin main  

Then go back to feature branch and merge:  
git checkout feature1  
git merge main  

This updates your branch with latest remote changes.  

________________________________________

## 4) If two developers edited the same file, what happens?

If both edited different lines, Git may merge automatically.  
If both edited the same line, Git shows a merge conflict.  

Example  

Developer 1 changed:  
print("Hello Team")  

Developer 2 changed:  
print("Hello DevOps")  

When merging, Git may show conflict markers like:  

<<<<<<< HEAD  
print("Hello Team")  
=======  
print("Hello DevOps")  
>>>>>>> feature-branch  

Then one developer must resolve it manually.  

________________________________________

## 5) If a merge conflict occurs, what steps will you follow?

Step 1: Pull or merge  
git pull origin main  
or  
git merge main  

Step 2: Open the conflicted file  
You will see:  

<<<<<<< HEAD  
print("Hello Team")  
=======  
print("Hello DevOps")  
>>>>>>> feature-branch  

Step 3: Edit and keep final correct code  
print("Hello DevOps Team")  

Step 4: Add resolved file  
git add app.py  

Step 5: Commit  
git commit -m "resolved merge conflict"  

Full example  
git checkout feature1  
git merge main  
# conflict happens  
# open file and fix it  
git add app.py  
git commit -m "resolved merge conflict with main"  

________________________________________

## 6) If you want to temporarily save your work, what will you use?

Use git stash.  

Save current uncommitted work  
git stash  

Check saved stashes  
git stash list  

Bring back the work  
git stash apply  
or  
git stash pop  

Example  
git stash  
git checkout main  
git pull origin main  
git checkout feature1  
git stash pop  

What it does  
It temporarily saves your changes so you can switch branches without committing unfinished work.  

________________________________________

## 7) If your push is rejected, what could be the reason?

Common reason  
Remote has new commits that your local branch does not have.  

Example error  
! [rejected] main -> main (fetch first)  

Fix steps  

Step 1: Pull latest changes  
git pull origin main  

Step 2: Resolve conflicts if any  

Step 3: Push again  
git push origin main  

________________________________________

Another possible reason  
Protected branch or no permission.  

Example:  
•	direct push to main is blocked  
•	you need to create a pull request instead  

________________________________________

## 8) If you want to undo the last commit but keep changes, what will you do?

Command  
git reset --soft HEAD~1  

Example  
git commit -m "added wrong file"  
git reset --soft HEAD~1  

Now the commit is removed, but changes are still available.  
Then fix and commit again:  
git commit -m "added correct file"  

________________________________________

## 9) If you want to discard all local changes, what will you do?

Discard uncommitted changes in tracked files  
git reset --hard HEAD  

Example  
git reset --hard HEAD  

This removes all uncommitted changes.  

________________________________________

If untracked files are also there  
git clean -fd  

Example  
git reset --hard HEAD  
git clean -fd  

What it does  
•	git reset --hard HEAD removes tracked file changes  
•	git clean -fd removes untracked files and folders  

Be careful, because this deletes local work.  

________________________________________

## 10) If you want to work on a new feature without affecting main code, what will you do?

Step 1: Create a new branch  
git checkout -b feature-login  

Step 2: Work on that branch  
git add .  
git commit -m "added login feature"  

Step 3: Push branch to remote  
git push origin feature-login  

Full example  
git checkout -b feature-login  
git add .  
git commit -m "added login API"  
git push origin feature-login  

This keeps main safe and isolated from your feature work.  

________________________________________

## 11) If you want to review someone else's code, what will you use?

Use a Pull Request.  

Practical steps  

Step 1: Developer pushes branch  
git push origin feature-login  

Step 2:  
In GitHub, open a Pull Request from feature-login to main  

Step 3:  
Reviewer checks:  
•	changed files  
•	commit history  
•	comments  
•	approval  

Step 4:  
After approval, merge PR  

Interview line  
“We usually use Pull Requests in GitHub to review code before merging into main.”  


## What is git reset vs git revert?

### 🟡 git reset
“Used to undo commits in my local repository by moving HEAD backward. It does not affect remote unless I force push, which is risky.”

________________________________________

### 🔴 git revert
“Used to safely undo a commit by creating a new commit. It is preferred when changes are already pushed because it does not rewrite history.”

### ✅ git reset

Works on LOCAL repo only (your commits)  

What it affects:  
•	Working directory ✔️ (depends on option)  
•	Local repo ✔️  
•	Remote repo ❌  

________________________________________

Example (not pushed yet)  
git commit -m "wrong commit"  
git reset --soft HEAD~1  

Now:  
•	commit removed from LOCAL repo  
•	changes still present in your files  
•	GitHub is NOT affected  

________________________________________

⚠️ Important:  
If you already pushed and then do reset:  
git reset --hard HEAD~1  
git push  

❌ Push will be rejected  
(because remote still has that commit)  

To force:  
git push --force  

⚠️ Dangerous in team environment  

________________________________________

### ✅ git revert

Works on BOTH local + remote (safe way)  

What it affects:  
•	Local repo ✔️  
•	Remote repo ✔️ (after push)  
•	History is preserved ✔️  

________________________________________

Example  
git revert <COMMIT ID>  
git push origin main  

Now:  
•	original commit is still there  
•	new commit is added that cancels it  






