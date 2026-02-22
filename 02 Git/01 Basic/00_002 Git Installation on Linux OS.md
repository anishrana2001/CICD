# 📘 Complete Guide: How to Install Git on Linux OS

**Author:** Instructor Guide
**Level:** Beginner to Advanced
**Platform:** Linux (Ubuntu, Debian, CentOS, RHEL, Fedora, Rocky, Amazon Linux)

---

# 📑 Table of Contents

1. What is Git?
2. Why Git is Required in Linux?
3. Prerequisites
4. Check if Git is Already Installed
5. Install Git on Ubuntu / Debian
6. Install Git on CentOS / RHEL / Rocky Linux
7. Install Git on Fedora
8. Install Git on Amazon Linux
9. Verify Installation
10. Configure Git First Time
11. Understanding Git Files in Linux
12. Test Git Installation
13. Install Specific Version of Git
14. Common Problems and Fix
15. Interview Questions
16. Summary

---

# 1️⃣ What is Git?

Git is a **Distributed Version Control System**.

It helps:

* Track code changes
* Collaborate
* Maintain version history
* Rollback changes

Linux kernel itself is managed using Git.

---

# 2️⃣ Why Git is Required in Linux?

Git is used in:

* DevOps
* Kubernetes
* Docker
* CI/CD
* GitHub
* GitLab

Linux is primary OS for DevOps engineers.

---

# 3️⃣ Prerequisites

Ensure you have:

✔ Linux machine
✔ Internet connection
✔ sudo access

Check sudo access:

```bash
sudo whoami
```

Output:

```bash
root
```

---

# 4️⃣ Check if Git is Already Installed

Run:

```bash
git --version
```

Output Example:

```bash
git version 2.34.1
```

**If installed** 👉 ❌ No need to install again.

**If not installed** 👉 ✅ Follow below steps.

---

# 5️⃣ Install Git on Ubuntu / Debian

## Step 1: Update Packages

```bash
sudo apt update
```

Output Example:

```bash
Reading package lists... Done
```

---

## Step 2: Install Git

```bash
sudo apt install git -y
```

Output Example:

```bash
Setting up git
```

---

# 6️⃣ Install Git on CentOS / RHEL / Rocky Linux. ![alt text](image-1.png)

## Step 1: Update

```bash
sudo yum update -y
```

OR (new version)

```bash
sudo dnf update -y
```

---

## Step 2: Install Git

```bash
sudo yum install git -y
```

OR

```bash
sudo dnf install git -y
```

Output:

```bash
Installed:
 git
```

---

# 7️⃣ Install Git on Fedora. ![alt text](image.png)

Run:

```bash
sudo dnf install git -y
```

---

# 8️⃣ Install Git on Amazon Linux

Run:

```bash
sudo yum install git -y
```

---

# 9️⃣ Verify Installation

Run:

```bash
git --version
```

Output:

```bash
git version 2.39.2
```

---

# 🔟 Configure Git First Time

This is required.

## Configure Username

```bash
git config --global user.name "Your Name"
```

Example:

```bash
git config --global user.name "Anish Rana"
```

---

## Configure Email

```bash
git config --global user.email "your@email.com"
```

Example:

```bash
git config --global user.email "anish@email.com"
```

---

## Verify

```bash
git config --list
```

Output:

```bash
user.name=Anish Rana
user.email=anish@email.com
```

---

# 1️⃣1️⃣ Understanding Git Files in Linux

When you run:

```bash
git init
```

Git creates:

```bash
.git directory
```

Check:

```bash
ls -la
```

Output:

```bash
.git
```

This folder contains:

* commits
* branches
* logs

This is Git database.

---

# 1️⃣2️⃣ Test Git Installation

## Step 1: Create Folder

```bash
mkdir git-test
```

---

## Step 2: Go inside

```bash
cd git-test
```

---

## Step 3: Initialize Git

```bash
git init
```

Output:

```bash
Initialized empty Git repository
```

---

## Step 4: Create File

```bash
touch file.txt
```

---

## Step 5: Add File

```bash
git add .
```

---

## Step 6: Commit

```bash
git commit -m "first commit"
```

Output:

```bash
1 file changed
```

---

# 1️⃣3️⃣ Install Specific Version of Git

Check available versions:

Ubuntu:

```bash
apt list -a git
```

Install specific:

```bash
sudo apt install git=2.34.1
```

---

# 1️⃣4️⃣ Common Problems and Fix

## Problem:

```
git command not found
```

Solution:

Install Git

---

## Problem:

Permission denied

Solution:

Use sudo

---

# 1️⃣5️⃣ Interview Questions

## Question 1

How to install Git in Ubuntu?

Answer:

```bash
sudo apt install git -y
```

---

## Question 2

Where Git stores data?

Answer:

```
.git directory
```

---

## Question 3

How to check Git version?

```bash
git --version
```

---

# 1️⃣6️⃣ Summary

You learned:

✔ Install Git in Linux
✔ Configure Git
✔ Verify Git
✔ Test Git
✔ Git directory structure

---

# 🚀 Next Topics

Recommended:

* Git clone
* Git push
* Git pull
* Git branch
* Git merge
* Git rebase
* Git stash

---

# ⭐ End of Document

---
