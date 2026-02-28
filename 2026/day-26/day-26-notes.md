
# Day 26 – GitHub CLI Notes

## What is GitHub CLI?

GitHub CLI (gh) allows developers to interact with GitHub directly from the terminal.

It can manage repositories, issues, pull requests, releases, and workflows without using the browser.

---

# Authentication Methods

GitHub CLI supports several authentication methods:

1. Browser authentication (recommended)
2. Personal Access Token (PAT)
3. SSH authentication
4. GitHub Enterprise authentication

---

# Working with Repositories

Using gh, we can:

Create repositories
Clone repositories
View repository information
Delete repositories
Open repositories in the browser

Example command:

gh repo create
gh repo clone
gh repo view
gh repo list

---

# Managing Issues

GitHub CLI allows issue management directly from terminal.

Commands:

gh issue create
gh issue list
gh issue view
gh issue close

This is useful for automation scripts that create issues automatically when errors occur.

---

# Managing Pull Requests

Pull requests can be created and merged without leaving the terminal.

Commands:

gh pr create
gh pr list
gh pr view
gh pr merge

Merge methods supported:

Merge commit
Squash merge
Rebase merge

---

# Reviewing PRs using gh

Developers can review PRs with:

gh pr checkout
gh pr review
gh pr diff

This allows code review directly in terminal.

---

# GitHub Actions Integration

GitHub CLI can interact with CI/CD workflows.

Commands:

gh run list
gh run view
gh workflow list

This allows engineers to monitor pipeline status without opening GitHub.

---

# How gh can be used in automation

GitHub CLI can be used in scripts to:

create issues automatically
trigger workflows
review pull requests
manage releases
fetch repository data using API

Example:

CI pipeline automatically creates an issue when a deployment fails.
```

---

# 8️⃣ Update `git-commands.md`

Add section:

````markdown
# GitHub CLI (gh)

## Authenticate

```bash
gh auth login
```

## Check authentication

```bash
gh auth status
```

## Create repository

```bash
gh repo create repo-name
```

## Clone repository

```bash
gh repo clone owner/repo
```

## List repositories

```bash
gh repo list
```

## View repository

```bash
gh repo view
```

## Create issue

```bash
gh issue create
```

## List issues

```bash
gh issue list
```

## Create pull request

```bash
gh pr create
```

## List pull requests

```bash
gh pr list
```

## Merge pull request

```bash
gh pr merge
```

## List workflow runs

```bash
gh run list
```

## View workflow run

```bash
gh run view <run-id>
```
````

---

# 9️⃣ Final Commit

```bash
git add .
git commit -m "Add Day 26 GitHub CLI notes and commands"
git push
```

---

# 🔥 DevOps Insight (Very Useful)

Many DevOps engineers automate GitHub using:

```
gh + bash scripts
gh + CI/CD pipelines
gh + GitHub Actions
```

Example automation:

```
Deployment fails
↓
Script runs
↓
gh issue create
↓
Slack alert
```

---
