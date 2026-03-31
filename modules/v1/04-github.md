# Module 4: GitHub

**Duration:** Week 4 | **Difficulty:** Beginner

---

> [!TIP]
> **Learning Tip:** GitHub is where you'll collaborate with other developers. By the end of this module, you'll know how to contribute to open source projects!

---

## Overview

GitHub is a platform for hosting Git repositories and collaborating with other developers. It provides tools for code review, project management, and continuous integration.

## Learning Objectives

By the end of this module, you will:

- [ ] Navigate GitHub interface
- [ ] Create and manage repositories
- [ ] Use GitHub for collaboration
- [ ] Implement GitHub Actions basics
- [ ] Contribute to open source

---

## Table of Contents

1. [GitHub Basics](#basics)
2. [Repository Management](#repository)
3. [Collaboration](#collaboration)
4. [Pull Requests](#pull-requests)
5. [GitHub Actions](#actions)
6. [Project Management](#projects)
7. [Exercises](#exercises)

---

## 1. GitHub Basics

### Creating an Account

1. Go to [github.com](https://github.com)
2. Sign up with email
3. Choose a plan (free tier works!)
4. Verify email

### GitHub Interface

```
┌─────────────────────────────────────────────┐
│  Logo    Search    +    Issues   PR   Code  │
├─────────────────────────────────────────────┤
│                                             │
│  Your Repositories                          │
│  ┌─────────────────────────────────────┐   │
│  │ repo-name                            │   │
│  │ Description...        Updated 2d    │   │
│  └─────────────────────────────────────┘   │
│                                             │
└─────────────────────────────────────────────┘
```

### Key Terms

| Term                  | Definition                               |
| --------------------- | ---------------------------------------- |
| **Repository (Repo)** | Project storage on GitHub                |
| **Fork**              | Copy someone else's repo to your account |
| **Star**              | Bookmark a repo                          |
| **Watch**             | Get notifications for activity           |
| **Gist**              | Share code snippets                      |

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 1:** What's the difference between cloning and forking a repository?
>
> - [ ] A) They are the same thing
> - [ ] B) Clone creates a local copy, fork creates a copy on GitHub under your account
> - [ ] C) Fork creates a local copy, clone creates a copy on GitHub
>
> <details>
> <summary>Answer</summary>
> **B) Clone creates a local copy, fork creates a copy on GitHub** - Forking is specifically for contributing to others' projects.
> </details>

---

## 2. Repository Management

### Creating a Repository

1. Click **+** → **New repository**
2. Enter name and description
3. Choose Public or Private
4. Add README (optional but recommended!)
5. Add .gitignore (optional)
6. Choose license (optional)
7. Click **Create repository**

> [!TIP]
> Always initialize with a README! It helps others understand your project.

### Repository Settings

| Setting               | Purpose                 |
| --------------------- | ----------------------- |
| **Rename**            | Change repo URL         |
| **Visibility**        | Public or Private       |
| **Collaborators**     | Add team members        |
| **Branch protection** | Require reviews         |
| **GitHub Pages**      | Deploy website          |
| **Webhooks**          | Trigger external events |

### README.md Template

```markdown
# Project Title

Short description.

## Features

- Feature 1
- Feature 2

## Getting Started

\`\`\`bash
npm install
npm start
\`\`\`

## Contributing

Guidelines...

## License

MIT
```

### GitHub Pages

1. Go to Settings → Pages
2. Select branch (usually `main`)
3. Choose folder (`/` or `/docs`)
4. Click Save
5. Your site is live at `username.github.io/repo`

> [!NOTE]
> Custom domain? Add it in Settings → Pages → Custom domain

---

## 3. Collaboration

### Inviting Collaborators

1. Go to Repository → Settings
2. Click **Collaborators**
3. Click **Add people**
4. Enter username/email
5. Send invitation

### Cloning

```bash
# Clone with HTTPS (easiest)
git clone https://github.com/user/repo.git

# Clone with SSH (requires setup)
git clone git@github.com:user/repo.git

# Clone specific branch
git clone -b develop https://github.com/user/repo.git
```

### Syncing Fork

```bash
# Add upstream remote
git remote add upstream https://github.com/original/repo.git

# Fetch upstream changes
git fetch upstream

# Merge into your branch
git checkout main
git merge upstream/main

# Or use rebase
git checkout main
git rebase upstream/main
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 2:** Why do you need an "upstream" remote when working with a fork?
>
> - [ ] A) You don't—it's optional
> - [ ] B) To fetch changes from the original repo to keep your fork updated
> - [ ] C) To push your changes to the original repo
>
> <details>
> <summary>Answer</summary>
> **B) To fetch changes from the original repo** - The upstream points to the original so you can stay in sync.
> </details>

---

## 4. Pull Requests

### Creating a Pull Request

1. Push your branch to GitHub
2. Click **Compare & pull request**
3. Fill in title and description
4. Add reviewers (optional)
5. Link issues (optional)
6. Click **Create pull request**

> [!TIP]
> Use GitHub's PR template if one exists—it helps reviewers understand your changes!

### PR Description Template

```markdown
## Description

Brief description of changes.

## Type of Change

- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Testing

How did you test this?

## Checklist

- [ ] Tests pass
- [ ] Code follows style
- [ ] Documentation updated
```

### Code Review

| Review Type         | Meaning                    |
| ------------------- | -------------------------- |
| **Approve**         | Ready to merge! ✓          |
| **Request Changes** | Needs fixes before merging |
| **Comment**         | Feedback/suggestion        |

```markdown
## Review Example

**Approve** - Looks good! Ship it! ✓

## Request Changes

Please address these issues:

1. Line 42: Use const instead of let
2. Function is too long, consider splitting
3. Missing error handling for edge case

## Comment

Question: Why did you choose this approach?
```

### Merging PRs

| Strategy         | When to Use                      |
| ---------------- | -------------------------------- |
| **Squash merge** | Keep history clean (recommended) |
| **Rebase merge** | Linear history                   |
| **Merge commit** | Preserve all commits             |

> [!WARNING]
> ⚠️ Always review changes before merging! Use branch protection rules to enforce this.

### Handling Conflicts

1. Click **Resolve conflicts**
2. Edit the conflicting files
3. Remove conflict markers
4. Mark as resolved
5. Click **Commit merge**

---

## 5. GitHub Actions

### Why CI/CD?

- ✅ Automated testing
- ✅ Catch bugs early
- ✅ Enforce code quality
- ✅ Deploy automatically

### Basic Workflow

Create `.github/workflows/main.yml`:

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: "20"
          cache: "npm"

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test

      - name: Build
        run: npm run build
```

> [!QUIZ]
>
> ### 🎯 Knowledge Check
>
> **Question 3:** What does the `on:` section in a workflow file define?
>
> - [ ] A) What the workflow does
> - [ ] B) When the workflow runs (triggers)
> - [ ] C) Who can run the workflow
>
> <details>
> <summary>Answer</summary>
> **B) When the workflow runs** - It specifies triggers like push, pull_request, etc.
> </details>

### Common Actions

```yaml
# Lint
- name: Run ESLint
  run: npm run lint

# Deploy to Netlify
- name: Deploy
  uses: nwtgck/actions-netlify@v2.0
  with:
    publish-dir: "./dist"
  env:
    NETLIFY_AUTH_TOKEN: ${{ secrets.NETLIFY_AUTH_TOKEN }}

# Store artifacts
- name: Upload artifact
  uses: actions/upload-artifact@v4
  with:
    name: build
    path: dist/
```

### Secrets

1. Go to Repository → Settings → Secrets and variables → Actions
2. Click **New repository secret**
3. Add name and value
4. Use in workflow: `${{ secrets.SECRET_NAME }}`

> [!WARNING]
> ⚠️ Never commit secrets to GitHub! Use secrets for API keys, tokens, etc.

---

## 6. Project Management

### Issues

Create issues to track:

- ✅ Bugs
- ✅ Feature requests
- ✅ Tasks

```markdown
## Issue Template

**Description**
Clear description of the issue.

**Steps to Reproduce**

1. Go to...
2. Click on...
3. See error

**Expected Behavior**
What should happen.

**Screenshots**
If applicable.

**Environment**

- OS: macOS 14
- Browser: Chrome 120
```

### Projects (Kanban)

1. Go to **Projects** → New project
2. Choose template (Kanban)
3. Add columns: To Do, In Progress, Done
4. Add cards from issues

### Milestones

1. Go to **Issues** → Milestones
2. Create milestone with due date
3. Add issues to milestone
4. Track progress

### Labels

| Label              | Color  | Use Case                |
| ------------------ | ------ | ----------------------- |
| `bug`              | Red    | Something isn't working |
| `enhancement`      | Green  | New feature request     |
| `documentation`    | Blue   | Docs improvements       |
| `help wanted`      | Purple | Need contributions      |
| `good first issue` | Green  | For beginners           |

---

## 7. Exercises

### ⭐ Exercise 1: Repository Setup

- [ ] Create a new GitHub repository
- [ ] Add a README with project description
- [ ] Enable GitHub Pages (try deploying a simple HTML site!)
- [ ] Go to Settings → Add a description and topics

---

### ⭐ Exercise 2: Fork and PR

- [ ] Find an interesting repo (maybe a library you use!)
- [ ] Fork it (click "Fork" button)
- [ ] Clone your fork locally
- [ ] Create a branch for your changes
- [ ] Make some improvements
- [ ] Push and create a pull request!

> [!TIP]
> Good first PRs: Fix typos, improve documentation, add tests

---

### ⭐ Exercise 3: GitHub Actions

- [ ] Create a workflow that runs `echo "Hello World"`
- [ ] Add a status badge to README
- [ ] Try adding a matrix strategy for multiple Node versions

---

### ⭐ Exercise 4: Project Board

- [ ] Create a project board
- [ ] Add 3 issues with different labels
- [ ] Move issues through columns
- [ ] Add a milestone with a due date

---

## GitHub CLI

```bash
# Install
brew install gh

# Login
gh auth login

# Create repo
gh repo create my-repo --public

# Clone repo
gh repo clone user/repo

# Create PR
gh pr create --title "Fix bug" --body "Fixes #123"

# Check PR status
gh pr status

# Create issue
gh issue create --title "Bug" --body "Description"
```

---

## Self-Check Questions

> [!QUIZ]
> Final check - can you answer these?

1. [ ] What's the difference between a fork and a clone?
2. [ ] How do you keep a fork synced with the original?
3. [ ] What is a pull request and when should you create one?
4. [ ] What are GitHub Actions used for?
5. [ ] How do you protect a branch from direct pushes?
6. [ ] What's the difference between issues and projects?
7. [ ] Why should you use secrets for API keys?

<details>
<summary>Click to reveal all answers</summary>

1. Fork = copy on GitHub, Clone = download to local
2. Add upstream remote, fetch, and merge/pull
3. Request to merge your changes into the main repo; for any significant changes
4. Continuous Integration/Deployment - automated testing, building, deployment
5. Go to Settings → Branches → Add rule → Require PR reviews
6. Issues = individual tasks/bugs, Projects = organize work across columns
7. They're encrypted and not stored in code; prevents exposure in public repos

</details>

---

## Resources

- [GitHub Docs](https://docs.github.com)
- [GitHub Skills](https://skills.github.com)
- [First Contributions](https://firstcontributions.github.io/) - Step-by-step guide
- [Awesome GitHub Actions](https://github.com/sdras/awesome-actions)

---

## Progress Checklist

- [ ] GitHub account created
- [ ] Exercise 1: Repository setup completed
- [ ] Exercise 2: Fork and PR completed
- [ ] Exercise 3: GitHub Actions tried
- [ ] Exercise 4: Project board created
- [ ] Answered all quiz questions
- [ ] Ready for Module 5: JavaScript Basics

---

## Next Module

[Module 5: JavaScript Basics →](../modules/05-javascript.md)

---

_Built with ❤️ by [Brian Riant](https://brianriant.vercel.app) - Ultimate Front-End Development_
