
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
