---

# 1️⃣ Commands You Should Run (Hands-On)

Inside your repo:

```bash
cd devops-git-practice
```

---

# Task 1 — Git Merge

### Create branch

```bash
git switch -c feature-login
```

Add some change

```bash
echo "Login feature" >> features.txt
git add features.txt
git commit -m "Add login feature"
```

Add another commit

```bash
echo "Login validation" >> features.txt
git add features.txt
git commit -m "Add login validation"
```

---

### Merge to main

```bash
git switch main
git merge feature-login
```

If **main had no new commits**, Git performs:

```
Fast-forward merge
```

---

### Create second branch

```bash
git switch -c feature-signup
```

Add commit

```bash
echo "Signup feature" >> features.txt
git add features.txt
git commit -m "Add signup feature"
```

---

### Add commit to main

```bash
git switch main
echo "Main update" >> features.txt
git add features.txt
git commit -m "Update main branch"
```

---

### Merge again

```bash
git merge feature-signup
```

This creates a:

```
Merge commit
```

---

# Task 2 — Git Rebase

### Create branch

```bash
git switch -c feature-dashboard
```

Add commits

```bash
echo "Dashboard UI" >> dashboard.txt
git add dashboard.txt
git commit -m "Add dashboard UI"
```

```bash
echo "Dashboard API" >> dashboard.txt
git add dashboard.txt
git commit -m "Add dashboard API"
```

---

### Move main forward

```bash
git switch main
echo "Main improvement" >> main.txt
git add main.txt
git commit -m "Improve main"
```

---

### Rebase

```bash
git switch feature-dashboard
git rebase main
```

View history

```bash
git log --oneline --graph --all
```

You will see **linear history**.

---

# Task 3 — Squash Merge

### Create branch

```bash
git switch -c feature-profile
```

Create multiple commits

```bash
echo "Profile page" >> profile.txt
git add profile.txt
git commit -m "Add profile page"
```

```bash
echo "Fix typo" >> profile.txt
git add profile.txt
git commit -m "Fix typo"
```

```bash
echo "Improve layout" >> profile.txt
git add profile.txt
git commit -m "Improve layout"
```

---

### Squash merge

```bash
git switch main
git merge --squash feature-profile
git commit -m "Add profile feature"
```

Result:

```
Only ONE commit added to main
```

---

### Regular merge example

```bash
git switch -c feature-settings
```

Add commits

```bash
echo "Settings page" >> settings.txt
git add settings.txt
git commit -m "Add settings page"
```

Merge normally

```bash
git switch main
git merge feature-settings
```

Now **all commits remain**.

---

# Task 4 — Git Stash

Make changes

```bash
echo "Temporary change" >> temp.txt
```

Try switching branch

```bash
git switch main
```

Git may warn about uncommitted changes.

---

### Stash work

```bash
git stash
```

Switch branch

```bash
git switch feature-login
```

Return

```bash
git switch main
```

Apply stash

```bash
git stash pop
```

---

### Multiple stashes

```bash
git stash push -m "work in progress"
git stash push -m "another change"
```

List stashes

```bash
git stash list
```

Example

```
stash@{0}
stash@{1}
```

Apply specific stash

```bash
git stash apply stash@{1}
```

---

# Task 5 — Cherry Pick

Create branch

```bash
git switch -c feature-hotfix
```

Add commits

```bash
echo "Fix bug A" >> hotfix.txt
git add hotfix.txt
git commit -m "Fix bug A"
```

```bash
echo "Fix bug B" >> hotfix.txt
git add hotfix.txt
git commit -m "Fix bug B"
```

```bash
echo "Fix bug C" >> hotfix.txt
git add hotfix.txt
git commit -m "Fix bug C"
```

---

Find commit hash

```bash
git log --oneline
```

Example

```
a31f1 Fix bug C
bc21d Fix bug B
d219c Fix bug A
```

Cherry-pick second commit

```bash
git switch main
git cherry-pick bc21d
```

Now only that commit appears in `main`.

---
