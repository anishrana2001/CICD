## 1️⃣ How to Create a Project in Git (Local Repository)

### What does it mean?

Creating a Git project means telling Git:

> “Please start tracking changes in this folder.”

### Step-by-step Example

```bash
mkdir student-app
cd student-app
git init
```

### What just happened?

* A hidden `.git` folder is created
* Git is now watching this directory

### Add your first file

```bash
echo "Hello Git" > app.txt
git status
git add app.txt
git commit -m "Initial commit"
```

📌 **Instructor Tip:**
A project without `git commit` is like a diary without saving pages.

---

