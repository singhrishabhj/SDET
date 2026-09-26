# Git Basics

## What is Git?

**Git** is a free and open-source **version control system (VCS)**.

It helps developers track changes made to files and code over time. It allows us to see what changed, go back to previous versions, work on different features, and collaborate with other developers.

---

## What is Version Control?

**Version control** is the management and tracking of changes made to documents, computer programs, websites, and other collections of information.

It helps us:

* Track changes over time
* Know what was changed and when
* Revert to an earlier version
* Work on multiple features
* Collaborate with other developers
* Maintain a history of the project

---

# Basic Terms

### Directory

A **directory** is simply another name for a **folder**.

Example:

```text
SDET/
├── Git/
├── Java/
├── Selenium/
└── API/
```

Here, `SDET` is a directory containing other directories.

---

### Terminal / CMD

A **Terminal** or **Command Prompt (CMD)** is an interface where we can execute commands using text.

For example:

```bash
cd SDET
```

---

### CLI

**CLI = Command Line Interface**

It allows us to interact with a computer or software by typing commands instead of using a graphical interface.

Git is commonly used through the CLI.

---

### `cd`

`cd` means **Change Directory**.

It is used to move from one folder to another.

Example:

```bash
cd SDET
```

Move one level back:

```bash
cd ..
```

---

### Code Editor

A **code editor** is a place where we write and edit code or other project files.

Examples:

* Visual Studio Code
* IntelliJ IDEA
* Sublime Text

---

### Repository

A **repository (repo)** is the place where a project and its Git version-control information are stored.

For example:

```text
SDET/
├── Git/
├── Java/
├── Selenium/
├── API/
└── .git/
```

The `.git` directory contains Git's internal information and history for that repository.

---

### GitHub

**GitHub** is an online platform used to host Git repositories.

It makes it easier to:

* Store repositories online
* Collaborate with other developers
* Share projects
* Review code
* Manage issues and pull requests
* Maintain project history

A simple way to remember it:

```text
Git = Version control system
GitHub = Online platform for hosting Git repositories
```

---

# Basic Git Commands

## 1. `git clone`

**Clone** brings a repository that is hosted somewhere, such as GitHub, onto your local machine.

Example:

```bash
git clone https://github.com/username/project.git
```

This creates a local copy of the repository.

Basic flow:

```text
GitHub Repository
       ↓
   git clone
       ↓
Local Machine
```

---

## 2. `git add`

`git add` tells Git which files or changes we want to include in the next commit.

Add a specific file:

```bash
git add filename
```

Add all changes:

```bash
git add .
```

Example:

```bash
git add LoginTest.java
```

or:

```bash
git add .
```

---

## 3. `git commit`

A **commit** saves a snapshot of the staged changes in Git's history.

Example:

```bash
git commit -m "Added login test"
```

The `-m` allows us to provide a message describing the changes.

Think of a commit as:

> **A saved checkpoint of your project.**

---

## 4. `git push`

`git push` uploads your local commits to a remote repository such as GitHub.

Example:

```bash
git push
```

First push of a new branch may look like:

```bash
git push -u origin main
```

Basic flow:

```text
Local Repository
      ↓
   git push
      ↓
GitHub Repository
```

---

## 5. `git pull`

`git pull` downloads changes from a remote repository and integrates them into your local repository.

Example:

```bash
git pull
```

Basic flow:

```text
GitHub Repository
      ↓
   git pull
      ↓
Local Repository
```

A simple way to remember:

```text
push = Local → Remote

pull  = Remote → Local
```

---

# `git status`

`git status` shows the current state of your working directory and staging area.

It can tell you about:

* New files
* Modified files
* Deleted files
* Untracked files
* Staged changes
* The current branch

Example:

```bash
git status
```

You may see:

```text
On branch main

Untracked files:
    login-test.java
```

This means Git can see the file, but it is not currently being tracked.

---

# Tracking a File with `git add`

Suppose we create:

```text
login-test.java
```

Git may show it as an **untracked file**.

To add it:

```bash
git add login-test.java
```

Then:

```bash
git status
```

The file will now appear under:

```text
Changes to be committed
```

This means the file has been moved into the **staging area**.

---

# Adding All Files

If we want to stage all changes in the current repository:

```bash
git add .
```

The `.` means:

> Add all applicable changes from the current directory.

---

# The Basic Git Workflow

This is one of the most important things to remember:

```text
                 Working Directory
                        │
                        │  git add
                        ↓
                  Staging Area
                        │
                        │  git commit
                        ↓
                Local Repository
                        │
                        │  git push
                        ↓
                  GitHub / Remote
```

For example:

```bash
# Check what changed
git status

# Stage changes
git add .

# Save changes in Git
git commit -m "Added Git notes"

# Upload commits to GitHub
git push
```

---

# Important Difference

### `git add`

Prepares changes for the next commit.

```text
"I want Git to include these changes."
```

### `git commit`

Creates a saved checkpoint in Git history.

```text
"Save these staged changes as a version."
```

### `git push`

Uploads those commits to GitHub.

```text
"Send my local commits to the remote repository."
```

So remember:

```text
git add
   ↓
Stage

git commit
   ↓
Save locally

git push
   ↓
Upload to GitHub
```

---

# Example

Suppose you create a new Selenium test:

```text
LoginTest.java
```

First:

```bash
git status
```

Git detects the new file.

Then:

```bash
git add LoginTest.java
```

Now the file is staged.

Then:

```bash
git commit -m "Added login test"
```

The change is now committed to your local Git repository.

Finally:

```bash
git push
```

The commit is uploaded to GitHub.

Complete flow:

```text
Create / Edit File
       ↓
git status
       ↓
git add
       ↓
git commit
       ↓
git push
       ↓
GitHub
```

---

# Quick Cheat Sheet

| Command                   | Purpose                                        |
| ------------------------- | ---------------------------------------------- |
| `git clone`               | Copy a remote repository to your local machine |
| `git status`              | Check the current state of your repository     |
| `git add file`            | Stage a specific file                          |
| `git add .`               | Stage all applicable changes                   |
| `git commit -m "message"` | Save staged changes as a commit                |
| `git push`                | Upload local commits to the remote repository  |
| `git pull`                | Download and integrate remote changes          |
| `git init`                | Create a new local Git repository              |
| `git branch`              | View/manage branches                           |
| `git merge`               | Combine changes from branches                  |

---

# The Mental Model

Think of Git like taking **checkpoints in a game**.

You make changes:

```text
Working Directory
```

You select what you want to save:

```text
git add
     ↓
Staging Area
```

You create a checkpoint:

```text
git commit
     ↓
Local Git History
```

You upload that checkpoint:

```text
git push
     ↓
GitHub
```

This workflow will become second nature once we practice it.
