# Module 3: Git Basics

**Duration:** Week 3 | **Difficulty:** Beginner

---

> [!TIP]
> **Learning Tip:** Git is best learned by doing! Follow along with each command in your terminal as you read.

---

## Overview

Git is a distributed version control system that tracks changes in your code. It's essential for collaboration and code management.

## Learning Objectives

By the end of this module, you will:

- [ ] Understand version control concepts
- [ ] Use Git commands for daily workflow
- [ ] Manage branches effectively
- [ ] Handle merge conflicts
- [ ] Work with remote repositories

---

## Table of Contents

1. [Introduction to Version Control](#introduction-to-version-control)
2. [Setting Up Git](#setting-up-git)
3. [Basic Commands](#basic-commands)
4. [Branching and Merging](#branching-and-merging)
5. [Remote Repositories](#remote-repositories)
6. [Advanced Commands](#advanced-commands)
7. [Exercises](#exercises)

---

## Introduction to Version Control

### What is Git?

Git tracks changes to your code over time. It allows you to:

- ✅ Save snapshots of your project
- ✅ Revert to previous versions
- ✅ Work on multiple features simultaneously
- ✅ Collaborate with others

> [!NOTE]
> Git was created by Linus Torvalds in 2005 for Linux kernel development.

### Key Concepts

| Term                  | Definition                     |
| --------------------- | ------------------------------ |
| **Repository (Repo)** | Project folder tracked by Git  |
| **Commit**            | A saved snapshot of changes    |
| **Branch**            | A separate line of development |
| **Merge**             | Combining branches together    |
| **Clone**             | Copying a repository           |

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 1:** What is a Git repository?
>
> - [ ] A) A type of code file
> - [ ] B) A project folder that Git tracks
> - [ ] C) A GitHub account
>
> <details>
> <summary>Answer</summary>
> **B) A project folder that Git tracks** - When you run `git init`, Git starts tracking that folder.
> </details>

---

## Setting Up Git

### Installation

```bash
# macOS (if not installed)
brew install git

# Linux (Debian/Ubuntu)
sudo apt-get install git

# Windows
# Download from git-scm.com
```

> [!TIP]
> Check if Git is installed: `git --version`

### Configuration

```bash
# Set your identity
git config --global user.name "Your Name"
git config --global user.email "your@email.com"

# Set default branch name
git config --global init.defaultBranch main

# Enable helpful features
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm commit

# List all settings
git config --list
```

> [!WARNING]
> ⚠️ Always set your name and email before making commits!

---

## Basic Commands

### Starting a Repository

```bash
# Create new repository
git init

# Clone existing repository
git clone https://github.com/user/repo.git
git clone https://github.com/user/repo.git my-folder
```

### Checking Status

```bash
# See current state
git status

# Shorthand status
git status -s

# See staged/unstaged changes
git diff

# See staged changes (what will be committed)
git diff --staged

# See commit history
git log
git log --oneline
git log --graph --decorate --all
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 2:** What command shows you which files have been changed but not yet staged?
>
> - [ ] A) `git log`
> - [ ] B) `git diff`
> - [ ] C) `git status`
>
> <details>
> <summary>Answer</summary>
> **B) `git diff`** - Shows unstaged changes. `git status` gives a broader overview.
> </details>

### Staging and Committing

```bash
# Stage specific file
git add filename.txt

# Stage all changes
git add .

# Stage all changes (shorthand)
git add -A

# Unstage file
git reset filename.txt
git reset

# Commit changes
git commit -m "Initial commit"

# Stage and commit in one command
git commit -am "Message"

# Amend last commit
git commit --amend
git commit --amend -m "New message"
```

> [!NOTE]
> The `-am` shortcut only works for modifying existing tracked files, not new files.

### Ignoring Files

Create `.gitignore`:

```gitignore
# Ignore node_modules
node_modules/

# Ignore build output
dist/
build/

# Ignore environment files
.env
.env.local

# Ignore log files
*.log

# Ignore IDE files
.vscode/
.idea/

# Ignore OS files
.DS_Store
Thumbs.db
```

> [!TIP]
> Common `.gitignore` templates: https://github.com/github/gitignore

---

## Branching and Merging

### Branch Commands

```bash
# List branches
git branch
git branch -a          # all branches (local + remote)
git branch -r         # remote branches only

# Create new branch
git branch feature-login

# Switch to branch
git checkout feature-login

# Create and switch (shorthand)
git checkout -b feature-login

# Rename branch
git branch -m old-name new-name
git branch -m main develop

# Delete branch (safe)
git branch -d feature-login

# Delete branch (force)
git branch -D feature-login
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 3:** What's the command to create AND switch to a new branch in one step?
>
> - [ ] A) `git branch new-branch`
> - [ ] B) `git checkout new-branch`
> - [ ] C) `git checkout -b new-branch`
>
> <details>
> <summary>Answer</summary>
> **C) `git checkout -b new-branch`** - Creates the branch and switches to it simultaneously.
> </details>

### Merging

```bash
# Merge branch into current
git merge feature-login

# Merge with no fast-forward
git merge --no-ff feature-login

# Rebase (alternative to merge)
git rebase main

# Abort rebase
git rebase --abort

# Continue rebase after resolving conflicts
git rebase --continue
```

> [!WARNING]
> ⚠️ **Rebase golden rule:** Never rebase commits that have been pushed to a shared repository!

### Handling Conflicts

```bash
# When conflict occurs
# 1. Edit the conflicted files
# 2. Remove conflict markers
# 3. Stage the resolved files
git add filename.txt
git commit

# Using merge tool
git mergetool
```

> [!TIP]
> Conflict markers look like:
>
> ```
> <<<<<<< HEAD
> Your changes
> =======
> Their changes
> >>>>>>> feature-branch
> ```

---

## Remote Repositories

### Remote Commands

```bash
# List remotes
git remote -v

# Add remote
git remote add origin https://github.com/user/repo.git
git remote add upstream https://github.com/original/repo.git

# Remove remote
git remote remove origin

# Rename remote
git remote rename origin upstream
```

### Fetching and Pulling

```bash
# Fetch changes (doesn't merge)
git fetch origin
git fetch --all

# Pull changes (fetch + merge)
git pull
git pull origin main
git pull --rebase origin main

# Force pull (overwrite local)
git fetch origin
git reset --hard origin/main
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 4:** What's the difference between `git fetch` and `git pull`?
>
> - [ ] A) They are the same
> - [ ] B) fetch downloads but doesn't merge, pull does both
> - [ ] C) pull downloads but doesn't merge, fetch does both
>
> <details>
> <summary>Answer</summary>
> **B) fetch downloads but doesn't merge, pull does both** - Pull = fetch + merge.
> </details>

### Pushing

```bash
# Push to remote
git push
git push origin main
git push origin feature-login

# Push and set upstream
git push -u origin feature-login
git push --set-upstream origin feature-login

# Push all branches
git push --all origin

# Delete remote branch
git push origin --delete feature-login

# Force push (use carefully!)
git push --force origin main
```

> [!WARNING]
> ⚠️ Never force push to main/master on shared repositories!

---

## Advanced Commands

### Stashing

```bash
# Save changes temporarily
git stash
git stash save "Work in progress"

# List stashes
git stash list

# Apply latest stash
git stash pop

# Apply specific stash
git stash apply stash@{2}

# Drop stash
git stash drop stash@{0}

# Clear all stashes
git stash clear
```

### Resetting

```bash
# Soft reset (keep changes staged)
git reset --soft HEAD~1

# Mixed reset (keep changes unstaged)
git reset --mixed HEAD~1

# Hard reset (discard changes)
git reset --hard HEAD~1

# Reset specific file
git checkout HEAD -- filename.txt
git reset HEAD -- filename.txt
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 5:** Which reset option discards all changes permanently?
>
> - [ ] A) `--soft`
> - [ ] B) `--mixed`
> - [ ] C) `--hard`
>
> <details>
> <summary>Answer</summary>
> **C) `--hard`** - This permanently deletes all changes! Use with caution.
> </details>

### Cherry-picking

```bash
# Apply specific commit
git cherry-pick abc123

# Cherry-pick without committing
git cherry-pick -n abc123
```

### Reflog (Recovery)

```bash
# See all reference updates
git reflog

# Recover lost commit
git checkout HEAD@{2}
git branch recovery HEAD@{2}
```

### Tagging

```bash
# Create lightweight tag
git tag v1.0.0

# Create annotated tag
git tag -a v1.0.0 -m "Version 1.0.0"

# Push tags
git push origin v1.0.0
git push --tags

# Delete local tag
git tag -d v1.0.0

# Delete remote tag
git push origin --delete v1.0.0
```

---

## Exercises

### ⭐ Exercise 1: Git Workflow

Follow these steps in your terminal:

- [ ] Create a new folder and navigate to it
- [ ] Run `git init` to initialize a repository
- [ ] Create `index.html` and commit: `git add . && git commit -m "Add index.html"`
- [ ] Create `style.css` and commit
- [ ] Check the history with `git log --oneline`

> [!TIP]
> Don't skip the `--oneline` flag—it makes the log much more readable!

---

### ⭐ Exercise 2: Branching

- [ ] Create a `feature` branch: `git checkout -b feature`
- [ ] Make commits on the feature branch
- [ ] Switch back to main: `git checkout main`
- [ ] Make a commit on main
- [ ] Merge feature into main: `git merge feature`

---

### ⭐ Exercise 3: Remote

- [ ] Create a GitHub repository (see next module!)
- [ ] Add remote: `git remote add origin <url>`
- [ ] Push your local repo: `git push -u origin main`

---

### ⭐ Exercise 4: Collaboration

- [ ] Clone a repo (or fork one)
- [ ] Create a new branch
- [ ] Make changes and commit
- [ ] Push and create a pull request (covered in next module!)

---

## Common Git Workflows

### Feature Branch Workflow

```
main → feature-branch → merge back to main
```

### Git Flow

```
main → develop → feature branches → release branches → hotfix
```

### Trunk-Based Development

```
main → short-lived branches → direct commit
```

---

## Self-Check Questions

> [!QUIZ]
> Final check - can you answer these?

1. [ ] How do you initialize a new Git repository?
2. [ ] What's the difference between `git add` and `git commit`?
3. [ ] How do you create and switch to a new branch?
4. [ ] What does `git pull` do vs `git fetch`?
5. [ ] How do you discard local changes?
6. [ ] What is a merge conflict and how do you resolve it?
7. [ ] Why should you avoid force pushing to shared branches?

<details>
<summary>Click to reveal all answers</summary>

1. `git init` - creates a .git folder in the current directory
2. `git add` stages changes, `git commit` saves them to history
3. `git checkout -b branch-name` - creates and switches in one command
4. `git pull` = fetch + merge, `git fetch` only downloads
5. `git reset --hard HEAD` or `git checkout -- file`
6. When same lines are modified in both branches; edit the file to resolve, then add and commit
7. It can overwrite others' work and break the shared history

</details>

---

## Resources

- [Git Documentation](https://git-scm.com/doc)
- [Atlassian Git Tutorial](https://www.atlassian.com/git/tutorials)
- [Oh Shit, Git!?!](https://ohshitgit.com/) - Common fixes!
- [Git Interactive Cheatsheet](https://ndpsoftware.com/git-cheatsheet.html)
- [Learn Git Branching](https://learngitbranching.js.org/) - Interactive!

---

## Progress Checklist

- [ ] Git installed and configured
- [ ] Exercise 1: Git workflow completed
- [ ] Exercise 2: Branching completed
- [ ] Exercise 3: Remote completed
- [ ] Answered all quiz questions
- [ ] Ready for Module 4: GitHub

---

## Next Module

[Module 4: GitHub →](./04-github.md)

---

_Built with ❤️ by [Brian Riant](https://brianriant.vercel.app) - Understanding For Every Developer_
