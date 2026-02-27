---
# 1️⃣ Hands-On Commands

## Task 1 — Git Reset

Create commits.

```bash
cd devops-git-practice
```

Commit A

```bash
echo "Commit A" >> reset.txt
git add reset.txt
git commit -m "Commit A"
```

Commit B

```bash
echo "Commit B" >> reset.txt
git add reset.txt
git commit -m "Commit B"
```

Commit C

```bash
echo "Commit C" >> reset.txt
git add reset.txt
git commit -m "Commit C"
```

Check history

```bash
git log --oneline
```

Example

```
c3 Commit C
b2 Commit B
a1 Commit A
```
---

## Reset Soft

```bash
git reset --soft HEAD~1
```

Result

```
Commit C removed
changes still staged
```

Check

```bash
git status
```

You will see the changes in **staging area**.

---

## Reset Mixed

Recommit first.

```bash
git commit -m "Commit C again"
```

Now run:

```bash
git reset --mixed HEAD~1
```

Result

```
Commit removed
changes moved to working directory
not staged
```

---

## Reset Hard

Recommit again.

```bash
git add .
git commit -m "Commit C again"
```

Now run:

```bash
git reset --hard HEAD~1
```

Result

```
Commit removed
changes deleted completely
```

---

# 2️⃣ Task 2 — Git Revert

Create commits

Commit X

```bash
echo "Commit X" >> revert.txt
git add revert.txt
git commit -m "Commit X"
```

Commit Y

```bash
echo "Commit Y" >> revert.txt
git add revert.txt
git commit -m "Commit Y"
```

Commit Z

```bash
echo "Commit Z" >> revert.txt
git add revert.txt
git commit -m "Commit Z"
```

Check history

```bash
git log --oneline
```

Example

```
z3 Commit Z
y2 Commit Y
x1 Commit X
```

---

## Revert Commit Y

Copy hash of commit Y.

```bash
git revert y2
```

Git creates a **new commit** that reverses Y.

Check log again.

```bash
git log --oneline
```

You will see:

```
r4 Revert "Commit Y"
z3 Commit Z
y2 Commit Y
x1 Commit X
```

Notice:

```
Commit Y still exists
```

---
