

---

# 1️⃣ Terminal Commands You Should Run

### Verify Git Installation

```bash
git --version
```

Example output

```
git version 2.43.0
```

---

### Configure Git Identity

```bash
git config --global user.name "Pawesha Shrestha"
git config --global user.email "pawesha@example.com"
```

Verify configuration

```bash
git config --list
```

---

### Create Git Project

```bash
mkdir devops-git-practice
cd devops-git-practice
```

Initialize Git repository

```bash
git init
```

Output

```
Initialized empty Git repository
```

---

### Check Repository Status

```bash
git status
```

Example

```
On branch main

No commits yet
nothing to commit
```

---

### Explore `.git` Directory

```bash
ls -la .git
```

Important folders:

```
.git/
├── HEAD
├── config
├── objects
├── refs
```

This folder **stores the entire Git history**.



# Day 22 – Git Notes

## 1. Difference between `git add` and `git commit`

`git add` moves changes from the working directory to the staging area.

`git commit` saves staged changes permanently in the Git repository with a message.

---

## 2. What does the staging area do?

The staging area allows you to prepare changes before committing them.

It lets developers select specific files or parts of files to include in a commit.

This helps create cleaner commit history.

---

## 3. What does `git log` show?

`git log` shows the commit history including:

- Commit hash
- Author name
- Date
- Commit message

It helps track project changes over time.

---

## 4. What is the `.git/` folder?

The `.git` folder contains all the data Git uses to manage the repository.

It includes:

- commit history
- branches
- configuration
- objects database

If the `.git` folder is deleted, the project will lose all version history and Git tracking.

---

## 5. Working Directory vs Staging Area vs Repository

Working Directory:
The current files you are editing.

Staging Area:
A temporary area where changes are prepared before committing.

Repository:
The permanent storage where committed changes are saved with history.


---

# 7️⃣ Expected Folder Structure

Your repo should look like this:

```
devops-git-practice
│
├── git-commands.md
├── day-22-notes.md
└── .git/
```

---

# 8️⃣ Final Commit

```bash
git add .
git commit -m "Add Day 22 Git notes and commands"
```

---

# 9️⃣ What Your Final `git log --oneline` Should Look Like

Example:

```
c72a1ab Add Day 22 Git notes and commands
9d321fa Update git commands documentation
61aa72c Add git diff and log commands
34bc901 Add initial git commands reference
```

---

# 🔟 DevOps Insight

Git is used everywhere:

| Tool       | Uses Git           |
| ---------- | ------------------ |
| GitHub     | code hosting       |
| GitLab     | CI/CD pipelines    |
| Jenkins    | builds from Git    |
| Docker     | pulls source code  |
| Kubernetes | GitOps deployments |

Git is **the backbone of DevOps workflows**.

---

