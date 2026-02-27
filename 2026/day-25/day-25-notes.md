
# Day 25 – Git Reset vs Revert & Branching Strategies

## 1. Difference between --soft, --mixed and --hard

git reset --soft
Moves the branch pointer back but keeps changes staged.

git reset --mixed
Moves the branch pointer back and unstages changes but keeps them in the working directory.

git reset --hard
Moves the branch pointer back and deletes changes completely.

---

## 2. Which reset is destructive?

git reset --hard is destructive because it permanently deletes commits and file changes.

---

## 3. When to use each reset type

--soft
Used when you want to change the last commit but keep changes staged.

--mixed
Used when you want to redo staging before committing again.

--hard
Used when you want to completely discard commits and changes.

---

## 4. Should you use reset on pushed commits?

No.

Reset rewrites history. If commits are already pushed, it can break other developers' history.

---


## ✅ 5. How `git revert` works

`git revert` **does NOT delete a commit**.

Instead, it:

* Takes the changes from a specific commit
* Creates a **new commit that reverses those changes**

👉 So history looks like:

```
A → B → C
```

If you revert commit **B**, Git adds a new commit:

```
A → B → C → D
```

Where:

* `D` = undo of `B`

📌 Example:

```bash
git revert <commit-hash>
```

---

## ✅ 6. Why `git revert` is safer

`git revert` is safe because:

### ✔️ 1. It keeps history intact

* No commits are deleted
* Everything remains traceable

### ✔️ 2. No history rewriting

* Unlike `git reset` or `git rebase`
* It does NOT change existing commits

### ✔️ 3. Safe for shared branches (VERY IMPORTANT)

If you're working with others:

* No conflicts due to history rewrite
* No force push needed

---

## ⚠️ Compare with `git reset` (important for interviews)

| Feature        | `git revert` | `git reset`     |
| -------------- | ------------ | --------------- |
| Deletes commit | ❌ No         | ✅ Yes (can)     |
| History safe   | ✅ Yes        | ❌ No (rewrites) |
| Shared branch  | ✅ Safe       | ❌ Dangerous     |
| New commit     | ✅ Yes        | ❌ No            |

---

## 💡 Simple way to remember

* `revert` = **"undo safely"**
* `reset` = **"erase history"**

---


## 7. When to use revert vs reset

Reset
Used locally before pushing commits.

Revert
Used on shared branches to undo changes safely.

---

# Reset vs Revert Comparison

| | git reset | git revert |
|---|---|---|
| What it does | Moves branch pointer backwards | Creates new commit that undoes change |
| Removes commit from history | Yes | No |
| Safe for shared branches | No | Yes |
| When to use | Local history cleanup | Undo commits in shared repository |

---



# 🚀 Branching Strategies (Refined + Practical)

## 1. GitFlow

A **structured and heavy workflow** designed for controlled releases.

### 🔁 Flow (real picture)

```
main (production)
  ↑
release (final testing)
  ↑
develop (integration branch)
  ↑
feature branches
```

### 🌿 Branch roles

* **main** → Production-ready code
* **develop** → Latest integrated features
* **feature/** → New features
* **release/** → Pre-release fixes, testing
* **hotfix/** → Emergency fixes from `main`

### 📦 Example flow

1. Create feature from `develop`
2. Merge feature → `develop`
3. Create `release` from `develop`
4. Test & fix → merge into `main`
5. Hotfix? → branch from `main`

---

### ✅ Pros

* Very **organized release cycle**
* Clear separation of environments

### ❌ Cons

* Complex for beginners
* Slower development speed
* Too heavy for startups

---

### 🧠 When to use

* Banking systems 💳
* Enterprise apps 🏢
* Fixed release cycles (monthly, quarterly)

---

## 2. GitHub Flow

A **simple and modern workflow** used by most teams today.

### 🔁 Flow

```
main
 ↑
feature branch
 ↑
pull request (code review + CI)
 ↑
merge → deploy 🚀
```

### 📦 Example flow

1. Create branch from `main`
2. Work on feature
3. Open Pull Request
4. Run tests + review
5. Merge → deploy immediately

---

### ✅ Pros

* Very simple ✅
* Fast delivery 🚀
* Easy for teams

### ❌ Cons

* Depends heavily on:

  * Testing 🧪
  * CI/CD ⚙️
* No built-in release structure

---

### 🧠 When to use

* Startups
* SaaS products
* Continuous deployment systems

---

## 3. Trunk-Based Development

The **fastest and most advanced approach**.

### 🔁 Flow

```
main (trunk)
 ↑
small short-lived branches (or direct commits)
 ↑
merge quickly (within hours/day)
```

### 📦 Key idea

* Branches live for **very short time**
* Frequent merges to `main`

---

### 🧠 Used by

* Google
* Meta

---

### ✅ Pros

* Very fast integration ⚡
* Fewer merge conflicts
* Always up-to-date codebase

### ❌ Cons

* Requires:

  * Strong automated testing 🧪
  * Feature flags 🚩
  * Discipline

---

### 🧠 When to use

* High-scale systems
* Strong DevOps culture
* Mature engineering teams

---

## 🌍 Real-world Example

* Kubernetes
  → Uses **trunk-based development with PRs**

---

# 🎯 Strategy Selection (Clear Decision Guide)

### 🚀 Startup (fast shipping)

👉 GitHub Flow OR Trunk-Based

* Fast releases
* Less process

---

### 🏢 Large enterprise (planned releases)

👉 GitFlow

* Stability
* Controlled releases

---

### ⚡ High-scale tech company

👉 Trunk-Based

* Speed + automation

---

# 🧠 Interview-Level Insight (Important)

👉 The **industry trend is moving away from GitFlow**

Why?

* Too slow for modern CI/CD
* Teams prefer:

  * GitHub Flow
  * Trunk-Based

---

# 💡 Simple Memory Trick

* **GitFlow** → “structured & slow”
* **GitHub Flow** → “simple & practical”
* **Trunk-Based** → “fast & advanced”

---




# 4️⃣ Update `git-commands.md`

Add this section.

````markdown
# Undoing Changes

## Reset soft
```bash
git reset --soft HEAD~1
```

## Reset mixed
```bash
git reset --mixed HEAD~1
```

## Reset hard
```bash
git reset --hard HEAD~1
```

## Revert commit
```bash
git revert <commit-hash>
```

## View reflog
```bash
git reflog
```

Shows history of HEAD movements and helps recover lost commits.
````

---

# 5️⃣ Commit & Push

```bash
git add .
git commit -m "Add Day 25 Git reset, revert and branching strategies notes"
git push
```

---

# 6️⃣ One DevOps Trick (Very Important)

If you accidentally run:

```
git reset --hard
```

You can recover commits using:

```bash
git reflog
```

Then restore:

```bash
git checkout <commit-hash>
```

or

```bash
git reset --hard <commit-hash>
```

This **saves developers all the time**.

---

