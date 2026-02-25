

---

# 1️⃣ Commands You Should Run (Hands-On)

Inside your repo:

```bash
cd devops-git-practice
```

---

## List all branches

```bash
git branch
```

Output example

```
* main
```

`*` shows the **current branch**.

---

## Create new branch

```bash
git branch feature-1
```

---

## Switch to branch

```bash
git switch feature-1
```

---

## Create + switch in one command

```bash
git switch -c feature-2
```

This does:

```
create branch
+
switch to it
```

---

## Switch back to feature-1

```bash
git switch feature-1
```

---

## Make a commit only on feature-1

Edit `git-commands.md`

Then run:

```bash
git add git-commands.md
git commit -m "Add branching commands documentation"
```

---

## Switch back to main

```bash
git switch main
```

Check history

```bash
git log --oneline
```

You will see that **feature-1 commit is not in main**.

That proves **branch isolation**.

---

## Delete branch

```bash
git branch -d feature-2
```

---

# 2️⃣ Push to GitHub

Create repo on GitHub called

```
devops-git-practice
```

Do **NOT add README**.

---

## Connect Local Repo to GitHub

```bash
git remote add origin https://github.com/USERNAME/devops-git-practice.git
```

Check remote

```bash
git remote -v
```

Example

```
origin https://github.com/user/devops-git-practice.git
```

---

## Push Main Branch

```bash
git push -u origin main
```

`-u` sets **upstream tracking**.

---

## Push feature branch

```bash
git push -u origin feature-1
```

Now GitHub will show:

```
main
feature-1
```

branches.

---

# 3️⃣ Pull Changes from GitHub

Edit a file on GitHub directly.

Then locally run:

```bash
git pull origin main
```

This downloads and merges the change.

---

# 4️⃣ Clone a Public Repository

Example:

```bash
git clone https://github.com/docker/cli.git
```

This downloads the repo.

---

# 5️⃣ Fork Repository

1. Go to GitHub repo
2. Click **Fork**
3. GitHub creates a copy in your account

Then clone your fork

```bash
git clone https://github.com/YOURNAME/repository-name.git
```

---
# 7️⃣ Add These to `git-commands.md`

Append:

````markdown
# Branching

## List branches
```bash
git branch
```

## Create branch
```bash
git branch feature-1
```

## Switch branch
```bash
git switch feature-1
```

## Create + switch branch
```bash
git switch -c feature-2
```

## Delete branch
```bash
git branch -d feature-2
```

---

# Remote Repositories

## Add remote
```bash
git remote add origin <repo-url>
```

## Show remote
```bash
git remote -v
```

## Push branch
```bash
git push origin branch-name
```

## Pull changes
```bash
git pull origin main
```

## Clone repository
```bash
git clone <repo-url>
```
````