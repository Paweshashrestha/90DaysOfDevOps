---

# 2️⃣ `git-commands.md`

Create this file inside the repo.

```bash
touch git-commands.md
```

Content:

````markdown
# Git Commands Reference

This document contains commonly used Git commands.

---

# Setup & Configuration

### Check Git version

```bash
git --version
```

Displays installed Git version.

---

### Set username

```bash
git config --global user.name "Your Name"
```

Sets the global Git username.

---

### Set email

```bash
git config --global user.email "your@email.com"
```

Sets the global Git email.

---

# Basic Workflow

### Initialize repository

```bash
git init
```

Creates a new Git repository.

---

### Add files to staging

```bash
git add file.txt
```

Adds a file to the staging area.

Add all files:

```bash
git add .
```

---

### Commit changes

```bash
git commit -m "Initial commit"
```

Saves staged changes to the repository.

---

# Viewing Changes

### Check repository status

```bash
git status
```

Shows modified, staged, and untracked files.

---

### View commit history

```bash
git log
```

Displays full commit history.

---

### Compact commit history

```bash
git log --oneline
```

Shows short commit history.

---

### Show changes in files

```bash
git diff
```

Displays changes not yet staged.

---

### Show staged changes

```bash
git diff --staged
```

Displays staged changes.

````

---

# 3️⃣ Stage and Commit

Stage file

```bash
git add git-commands.md
```

Check staging

```bash
git status
```

Commit

```bash
git commit -m "Add initial git commands reference"
```

---

# 4️⃣ Make More Commits

Edit the file and add more commands.

Check changes

```bash
git diff
```

Commit again

```bash
git add git-commands.md
git commit -m "Add git diff and log commands"
```

Repeat again

```bash
git add git-commands.md
git commit -m "Update git commands documentation"
```

---

# 5️⃣ View Commit History

```bash
git log --oneline
```

Example output

```
a72c1a2 Update git commands documentation
9a21c4f Add git diff and log commands
3b91e8d Add initial git commands reference
```


---
````
