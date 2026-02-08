## 1️⃣ Git Rebase – Clean Commit History

### What is Rebase?

`git rebase` means:

> Rewriting commit history by moving your commits on top of another branch

Think of it as:

> "Replay my work on the latest code"

---

### Why Rebase is Used (Industry Reason)

* Keeps commit history **linear and clean**
* Easier for code reviews
* Preferred before merging feature branches

---

### Real Student Example 🇮🇳

* You created `feature-login`
* Meanwhile, `main` branch received new commits
* Your branch is outdated

Instead of merging (which creates extra commits), you **rebase**.

---

### Practical Example

```bash
git checkout feature-login
git rebase main
```

What happens internally:

* Your commits are temporarily removed
* Latest `main` commits are applied
* Your commits are replayed on top

---

### Rebase Conflict Resolution

If conflict occurs:

```bash
# fix the file
git add conflicted-file.txt
git rebase --continue
```

To cancel rebase:

```bash
git rebase --abort
```

📌 **Instructor Rule:**
Never rebase a branch that is already shared with the team.

---

## 2️⃣ Git Stash – Temporary Shelf for Code

### What is Stash?

`git stash` means:

> Save unfinished work temporarily and clean your working directory

---

### Real-Life Scenario 🇮🇳

You are coding and your manager says:

> "Fix production issue NOW"

You cannot commit half-done work.

👉 **Use stash**

---

### Basic Stash Commands

```bash
git stash
```

Your working directory becomes clean.

Bring changes back:

```bash
git stash apply
```

---

### Multiple Stashes

```bash
git stash list
git stash apply stash@{0}
```

Remove stash after applying:

```bash
git stash pop
```

---

📌 **Instructor Tip:**
Stash is temporary. Do NOT use it as long-term storage.

---

## 3️⃣ Git Reset – Move HEAD Pointer (Dangerous)

### What is Reset?

`git reset` means:

> Move your branch pointer to an earlier commit

⚠️ This can **rewrite history**.

---

### Types of Reset (Very Important)

| Reset Type        | Effect                              |
| ----------------- | ----------------------------------- |
| --soft            | Keeps file changes & staging        |
| --mixed (default) | Keeps file changes, removes staging |
| --hard            | Deletes everything                  |

---

### Example

```bash
git reset --soft HEAD~1
```

Undo last commit but keep changes staged.

```bash
git reset --hard HEAD~1
```

Completely remove last commit and changes.

---

📌 **Golden Warning:**
Never use `git reset --hard` on shared branches.

---

## 4️⃣ Git Revert – Safe Undo (Recommended)

### What is Revert?

`git revert` means:

> Create a new commit that reverses an earlier commit

---

### Why Revert is Safer

* Does NOT rewrite history
* Safe for shared branches
* Preferred in production

---

### Practical Example

```bash
git revert commit_id
```

Git creates a **new commit** that undoes the changes.

---

### Industry Scenario 🇮🇳

* Buggy code deployed to production
* Cannot change history

👉 Use **git revert**, not reset

---

## 5️⃣ Reset vs Revert – Clear Comparison

| Feature                | git reset      | git revert       |
| ---------------------- | -------------- | ---------------- |
| History rewrite        | ✅ Yes          | ❌ No             |
| Safe for shared branch | ❌ No           | ✅ Yes            |
| Creates new commit     | ❌ No           | ✅ Yes            |
| Use case               | Local mistakes | Production fixes |

---

## 6️⃣ Rebase vs Merge (Interview Favorite)

| Feature                | Rebase           | Merge         |
| ---------------------- | ---------------- | ------------- |
| Commit history         | Linear           | Non-linear    |
| Extra merge commit     | ❌ No             | ✅ Yes         |
| Safe for shared branch | ❌ No             | ✅ Yes         |
| Preferred              | Feature branches | Main branches |

---

## 7️⃣ Common Mistakes Students Make

❌ Rebasing `main` branch
❌ Using `reset --hard` blindly
❌ Forgetting stashed changes
❌ Rewriting shared history

---

## 8️⃣ Practice Lab (Highly Recommended)

1. Create a feature branch
2. Make 2 commits
3. Rebase onto main
4. Create conflict and resolve
5. Use stash during branch switch
6. Undo a commit using reset
7. Undo another using revert

---

## 9️⃣ Interview One-Liners

* Rebase rewrites history, merge does not
* Reset is dangerous, revert is safe
* Stash is a temporary shelf
* Never rebase shared branches

---

## 🔟 Final Takeaway

> Advanced Git is about **control and discipline**, not just commands.

Learn carefully → Practice safely → Use confidently

---

✅ This guide is designed for **hands-on classroom & industry readiness**
