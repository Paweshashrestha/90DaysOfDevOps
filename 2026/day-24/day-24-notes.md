# Day 24 – Advanced Git Notes

## 1. What is a fast-forward merge?

A fast-forward merge happens when the main branch has no new commits after a feature branch was created.

Git simply moves the branch pointer forward without creating a merge commit.

---

## 2. When does Git create a merge commit?

Git creates a merge commit when both branches have new commits.

This preserves the branch history.

---

## 3. What is a merge conflict?

A merge conflict occurs when the same line of the same file is modified in two different branches.

Git cannot automatically decide which version to keep, so the developer must resolve it manually.

---



## 4. What does rebase do?

Rebase **moves your branch to start from another base (usually latest main branch)**.

👉 Instead of merging, it **replays your commits on top of the latest code**.

### Simple idea:

* Old:

  ```
  A---B---C (main)
       \
        D---E (feature)
  ```

* After rebase:

  ```
  A---B---C---D'---E' (feature)
  ```

✔ Your commits (D, E) are **rebuilt** on top of C
✔ History becomes **straight (linear)**
❗ Commit IDs change (because history is rewritten)

---

## 5. How is rebase history different from merge?

### 🔹 Merge

* Keeps original history
* Adds a **merge commit**
* Shows **true branch structure**

```
A---B---C--------F (main)
     \          /
      D---E---- (feature)
```

✔ Safe
✔ No history rewrite
❌ History looks messy

---

### 🔹 Rebase

* Rewrites history
* No merge commit
* Creates a **clean linear timeline**

```
A---B---C---D'---E' (feature)
```

✔ Clean & easy to read
✔ Looks like work was done sequentially
❗ Can be dangerous if used on shared branches

---

## 🔥 One-line difference (interview gold)

👉 **Merge preserves history, Rebase rewrites history to make it linear**

---

## ⚠️ Important rule (VERY important)

👉 Never rebase shared/public branches (like `main`)

Why?

* It changes commit history
* Breaks other people’s work

---


## 6. Why should you not rebase shared commits?

Rebasing rewrites commit history.

If commits are already pushed and shared, rebasing can cause conflicts and confusion for other developers.

---

## 7. When to use rebase vs merge?

Merge:
Used for preserving branch history and collaboration.

Rebase:
Used for cleaning up local commit history before merging.

---

## 8. What does squash merging do?

Squash merging combines multiple commits from a branch into a single commit before merging.

---

## 9. When would you use squash merge?

When a feature branch contains many small commits and you want a clean history.

---

## 10. Trade-off of squashing

You lose the detailed commit history of the feature branch.

---

## 11. Difference between stash pop and stash apply

git stash pop:
Applies the stash and removes it from stash list.

git stash apply:
Applies the stash but keeps it in the stash list.

---

## 12. When is stash used?

Stash is used when you have unfinished changes but need to quickly switch branches.

---

## 13. What does cherry-pick do?

Cherry-pick applies a specific commit from one branch to another.

---

## 14. When would cherry-pick be used?

Cherry-pick is useful for applying a specific bug fix to another branch without merging the entire branch.

---

## 15. What can go wrong with cherry-picking?

Cherry-picking can duplicate commits and create conflicts if the code has changed significantly.

`````


## Rebase branch

```bash
git rebase main
```

## View history graph

```bash
git log --oneline --graph --all
```

## Squash merge

```bash
git merge --squash branch-name
```

## Stash changes

```bash
git stash
```

## Stash with message

```bash
git stash push -m "message"
```

## List stashes

```bash
git stash list
```

## Apply stash

```bash
git stash apply
```

## Apply and remove stash

```bash
git stash pop
```

## Cherry pick commit

```bash
git cherry-pick <commit-hash>
```

````

---


# 5️⃣ Real DevOps Insight

Most companies follow this flow:

```
feature branch
     ↓
rebase with main
     ↓
pull request
     ↓
squash merge
     ↓
main branch
```

Tools that depend on this workflow:

* GitHub
* GitLab
* Jenkins
* Kubernetes GitOps
* ArgoCD

---
````
