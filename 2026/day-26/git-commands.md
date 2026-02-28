---

# 1️⃣ Task 1 — Install GitHub CLI

### Install on Linux (Ubuntu / Debian)

```bash
sudo apt update
sudo apt install gh
```

### Mac (Homebrew)

```bash
brew install gh
```

### Windows

```bash
winget install GitHub.cli
```

---

## Authenticate with GitHub

```bash
gh auth login
```

Follow prompts:

```
GitHub.com
HTTPS
Login with browser
```

---

## Verify login

```bash
gh auth status
```

Example output:

```
Logged in to github.com as pawesha
Git operations protocol: https
Token: ✓
```

---

# 2️⃣ Task 2 — Working with Repositories

## Create a repo from terminal

```bash
gh repo create test-gh-cli \
--public \
--description "Testing GitHub CLI" \
--clone \
--add-readme
```

This will:

```
Create repo
Clone repo
Add README
```

---

## Clone repo using gh

Instead of:

```bash
git clone repo-url
```

Use:

```bash
gh repo clone owner/repo
```

Example

```bash
gh repo clone pawesha/devops-git-practice
```

---

## View repository details

```bash
gh repo view
```

Shows:

```
description
stars
forks
issues
pull requests
```

---

## List all repositories

```bash
gh repo list
```

Example

```
devops-git-practice
kubernetes-notes
linux-lab
```

---

## Open repo in browser

```bash
gh repo view --web
```

or

```bash
gh browse
```

---

## Delete repo

⚠️ Careful with this.

```bash
gh repo delete test-gh-cli
```

GitHub asks confirmation.

---

# 3️⃣ Task 3 — Issues

## Create issue

```bash
gh issue create \
--title "Bug in login page" \
--body "Login validation not working" \
--label bug
```

---

## List issues

```bash
gh issue list
```

---

## View specific issue

```bash
gh issue view 1
```

---

## Close issue

```bash
gh issue close 1
```

---

# 4️⃣ Task 4 — Pull Requests

## Create branch

```bash
git switch -c feature-cli-demo
```

Add change

```bash
echo "Testing GitHub CLI" >> cli.txt
git add cli.txt
git commit -m "Add CLI test file"
git push origin feature-cli-demo
```

---

## Create PR from terminal

```bash
gh pr create --fill
```

This auto-fills:

```
PR title
PR description
```

---

## List pull requests

```bash
gh pr list
```

---

## View PR details

```bash
gh pr view
```

Shows:

```
reviewers
checks
CI status
commits
```

---

## Merge PR

```bash
gh pr merge
```

---

### Merge options

```bash
gh pr merge --merge
```

Regular merge

```
Merge commit
```

---

```bash
gh pr merge --squash
```

Squash commits

---

```bash
gh pr merge --rebase
```

Rebase commits

---

# 5️⃣ Task 5 — GitHub Actions Preview

## List workflow runs

```bash
gh run list
```

Example

```
CI build
Deploy pipeline
Lint checks
```

---

## View workflow run

```bash
gh run view <run-id>
```

Example

```bash
gh run view 123456789
```

Shows

```
status
logs
jobs
steps
```

---

# 6️⃣ Useful gh Tricks

## GitHub API access

```bash
gh api repos/owner/repo
```

Example

```bash
gh api repos/kubernetes/kubernetes
```

---

## Create Gist

```bash
gh gist create file.txt
```

Private gist:

```bash
gh gist create file.txt --private
```

---

## Create release

```bash
gh release create v1.0.0
```

Add notes:

```bash
gh release create v1.0.0 --notes "First release"
```

---

## Create alias

Example:

```bash
gh alias set prs "pr list"
```

Now run:

```bash
gh prs
```

---

## Search repos

```bash
gh search repos kubernetes
```

---