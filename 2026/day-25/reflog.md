---

# 🔥 Recovering from `git reset --hard`

## ❗ What `git reset --hard` does

```bash
git reset --hard
```

* Deletes **uncommitted changes**
* Moves HEAD to another commit
* Can make commits “disappear” from normal history

👉 Feels like data is gone… but it’s usually NOT 😎

---

# 🧠 The Lifesaver: `git reflog`

```bash
git reflog
```

### What it does:

- Shows **every movement of HEAD**
- Includes:
  - commits
  - resets
  - checkouts

👉 Even “deleted” commits are still here temporarily

---

## 📦 Example Scenario

Before mistake:

```id="6ly5d7"
A → B → C → D (HEAD)
```

You run:

```bash
git reset --hard B
```

Now:

```id="9t19fu"
A → B (HEAD)
```

😱 Commits C and D look lost

---

## ✅ Recovery Steps

### 1. Run:

```bash
git reflog
```

Output example:

```id="ezld5x"
a1b2c3 HEAD@{0}: reset: moving to B
d4e5f6 HEAD@{1}: commit: Added feature (D)
c7g8h9 HEAD@{2}: commit: Fixed bug (C)
```

---

### 2. Restore commit

#### Option 1 (safe):

```bash
git checkout d4e5f6
```

- Moves to that commit (detached HEAD)

---

#### Option 2 (full restore):

```bash
git reset --hard d4e5f6
```

👉 Now history is back:

```id="iyv0w9"
A → B → C → D (HEAD)
```

---

# ⚠️ Important Notes

### ⏳ Reflog is temporary

- Usually stored for ~30–90 days
- After that → commits may be garbage collected

---

### ❌ Won’t help if:

- Repo is deleted
- Garbage collection already removed commits

---

# 💡 Pro Tip (Very Important)

Before risky commands:

```bash
git branch backup
```

👉 Creates a safety checkpoint

---

# 🧠 Interview-Level Insight

👉 **Difference:**

- `git log` → shows commit history
- `git reflog` → shows **all HEAD movements (including hidden ones)**

---

# 🔑 One-Line Memory

👉 **reflog = Git’s “undo history”**

---
