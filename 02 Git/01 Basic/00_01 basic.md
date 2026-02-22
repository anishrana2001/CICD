# 🧑‍🏫 Hands-on Git & GitLab – Instructor Guide (with Practical Examples)

## **Audience:** Students, freshers, beginners
### **Goal:** Learn Git by *`doing`*, not memorizing commands

---

## 0️⃣ Step‑by‑Step Installation For Windows (10 minutes)

## Step 1: Download Git

Open browser and go to:

```
https://git-scm.com/install/windows
```

Download the file as per your requirement.

You will get file like:

```
Git-2.xx.x-64-bit.exe
```

---

## Step 2: Run Installer

Double click:

```
Git-2.xx.x-64-bit.exe
```

Click:

```
Next
```

---

## Step 3: Select Components

Keep **default settings**

Click:

```
Next
```

---

## Step 4: Choose Default Editor

Recommended:

Select:

```
Use Visual Studio Code
```

or

```
Use Notepad
```

Click:

```
Next
```

---

## Step 5: Adjust PATH Environment

IMPORTANT STEP ⚠️

Select:

```
Git from the command line and also from 3rd‑party software
```

Click:

```
Next
```

---

## Step 6: Choose HTTPS Transport

Select:

```
Use the OpenSSL library
```

Click:

```
Next
```

---

## Step 7: Configure Line Ending

Select:

```
Checkout Windows-style, commit Unix-style
```

Click:

```
Next
```

---

## Step 8: Terminal Emulator

Select:

```
Use MinTTY
```

Click:

```
Next
```

---

## Step 9: Default Options

Keep everything default

Click:

```
Install
```

Installation will start.

Wait for completion.

Click:

```
Finish
```

---

# 5️⃣ Verify Git Installation

Now verify Git.

## Method 1: Using CMD

Open:

```
Command Prompt
```

Run:

```bash
git --version
```

## Output Example:

```bash
git version 2.44.0.windows.1
```

This means Git is successfully installed.

---

## Method 2: Using Git Bash

Open:

```
Git Bash
```

Run:

```bash
git --version
```

---

# 6️⃣ First‑Time Git Configuration

This is VERY IMPORTANT.

Git needs:

* Username
* Email

## Set Username

```bash
git config --global user.name "Your Name"
```

Example:

```bash
git config --global user.name "Anish Rana"
```

---

## Set Email

```bash
git config --global user.email "your@email.com"
```

Example:

```bash
git config --global user.email "anishrana2001@rediffmail.com"
```

---

## Verify Configuration

```bash
git config --list
```

Output:

```bash
user.name=Anish Rana
user.email=anish@email.com
```

---

# 7️⃣ Understanding Git Bash

Git Bash is a terminal.

It supports:

* Linux commands
* Git commands

Example:

```bash
pwd
ls
mkdir project
cd project
```

---


---

