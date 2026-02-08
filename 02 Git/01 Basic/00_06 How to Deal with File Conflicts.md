## 6️⃣ How to Deal with File Conflicts

### What is a conflict?

A conflict happens when:

* Two people edit the **same line** of the same file
* Git doesn’t know which version to keep

### Real Team Scenario 🇮🇳

* Student A edits `app.txt`
* Student B edits same line
* Both push → 💥 conflict

### Conflict Example

```text
<<<<<<< HEAD
Hello from Student A
=======
Hello from Student B
>>>>>>> feature-branch
```

### How to Resolve

1. Open the file
2. Decide correct content
3. Remove conflict markers
4. Save file

```bash
git add app.txt
git commit -m "Resolved merge conflict"
```

📌 **Instructor Tip:**
Conflicts are normal — panic is optional 😄

---

## 7️⃣ Difference Between git push and git pull

| Command  | Meaning         | Direction      |
| -------- | --------------- | -------------- |
| git push | Send code       | Local → GitLab |
| git pull | Get latest code | GitLab → Local |

### git pull internally does:

```bash
git fetch + git merge
```

### When to use git pull?

* Before starting work
* Before pushing code

📌 **Golden Rule:**
Always `git pull` before `git push`

---

## 8️⃣ Additional Important Topics (Must-Know)

### 🔹 git status

Shows current state of files

```bash
git status
```

---

### 🔹 git log

Shows commit history

```bash
git log --oneline
```

---

### 🔹 .gitignore

Used to ignore files like:

* passwords
* build files
* node_modules

Example `.gitignore`:

```text
node_modules/
.env
*.log
```

---

## 9️⃣ Recommended Practice Task (Homework)

1. Create a local Git project
2. Push it to GitLab
3. Create a feature branch
4. Make changes
5. Merge to main
6. Intentionally create a conflict and resolve it

---

## 🔟 Final Summary

* Git tracks changes
* GitLab stores & shares code
* Branching keeps code safe
* Conflicts are part of teamwork

> **Learn Git once → Use everywhere (GitHub, GitLab, Bitbucket)**

---

✅ This guide is designed for **hands-on learning**, not rote commands
