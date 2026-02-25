# Day 23 – Git Branching & GitHub

## 1. What is a branch in Git?

A branch is an independent line of development in Git.  
It allows developers to work on features or fixes without affecting the main codebase.

---

## 2. Why do we use branches instead of committing everything to main?

Branches allow safe development.

Benefits:

- isolate features
- avoid breaking production code
- allow multiple developers to work simultaneously
- enable testing before merging to main

---

## 3. What is HEAD in Git?

HEAD is a pointer that indicates the current branch or commit you are working on.

When you switch branches, HEAD moves to that branch.

---

## 4. What happens to your files when switching branches?

Git changes the working directory to match the files stored in that branch.

Files may:

- appear
- disappear
- change versions

depending on the branch history.

---

### 🔹 Difference Between **origin** and **upstream**

#### ✅ **origin**

- This is the **default remote** linked to your local repository.
- Usually points to **your own repo (your fork or your repo)**.
- You **push and pull** code here.

👉 Think: _“My copy”_

---

#### ✅ **upstream**

- Refers to the **original repository** you forked from.
- Used to **sync updates from the main project**.
- You usually **pull from upstream**, not push.

👉 Think: _“Original source”_

---

### 🔁 Example Workflow

```
Original Repo (upstream)
        ↓
     Fork Repo (origin)
        ↓
   Your Local Repo
```

---

### 🔧 Common Commands

- Add upstream:

```bash
git remote add upstream <repo-url>
```

- Fetch latest from upstream:

```bash
git fetch upstream
```

- Merge into your branch:

```bash
git merge upstream/main
```

---

### 🧠 Easy Way to Remember

- **origin = yours**
- **upstream = original**

---

---

## 6. Difference between git fetch and git pull

git fetch:
Downloads changes from remote repository but does not merge them.

git pull:
Fetches changes and merges them into the current branch.

---

## 7. Clone vs Fork

Clone:
Creates a local copy of a repository.

Fork:
Creates a copy of a repository in your GitHub account.

---

## 8. When do we use clone vs fork?

Clone:
When working on your own project or when you have direct access.

Fork:
When contributing to someone else's project.

---

## 9. How to keep a fork updated with original repo?

1. Add upstream remote:

git remote add upstream https://github.com/original/repo.git

2. Fetch changes:

git fetch upstream

3. Merge changes:

git merge upstream/main


# 🔟 Real DevOps Workflow Example

In real companies:

```

main
│
├── feature-login
├── feature-payment
├── bugfix-auth

```

Developers:

```

create branch
work
commit
push
open pull request
merge to main

```

---

```
