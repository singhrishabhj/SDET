# Git Push, Pull, Remote & Upstream

## 1. What is a Remote Repository?

A **remote repository** is the Git repository hosted somewhere outside your local machine.

For example:

```text
Local Machine
     │
     │
     ↓
   SDET/
     │
     │
     ↓
GitHub Repository
```

Our local repository and GitHub repository are separate.

Git allows us to synchronize them.

---

# 2. What is `origin`?

When we connect our local repository to GitHub, we normally give the remote repository the name:

```text
origin
```

Example:

```bash
git remote add origin git@github.com:USERNAME/SDET.git
```

Here:

```text
origin = name of the remote
```

It is just a conventional name. We could technically call it something else, but `origin` is the standard convention.

Check your remote:

```bash
git remote -v
```

Example:

```text
origin  git@github.com:USERNAME/SDET.git (fetch)
origin  git@github.com:USERNAME/SDET.git (push)
```

---

# 3. What is `git push`?

`git push` sends commits from your **local repository** to the **remote repository**.

Basic flow:

```text
Local Working Directory
          ↓
       git add
          ↓
       git commit
          ↓
    Local Repository
          ↓
       git push
          ↓
      GitHub
```

Example:

```bash
git push
```

---

# 4. First Push

When pushing a branch for the first time, Git may not know which remote branch it should be connected to.

For example:

```bash
git push -u origin main
```

Break it down:

```text
git push
```

Push commits.

```text
origin
```

Push to the remote named `origin`.

```text
main
```

Push the local `main` branch.

```text
-u
```

Set the upstream/tracking relationship.

---

# 5. What is Upstream?

An **upstream branch** is the remote branch that your local branch is configured to track.

For example:

```text
Local branch                Remote branch

main        ─────────────→  origin/main
```

If we run:

```bash
git push -u origin main
```

Git remembers:

```text
main → origin/main
```

After that, while you're on `main`, you can usually simply run:

```bash
git push
```

instead of:

```bash
git push origin main
```

The `-u` is short for:

```text
--set-upstream
```

So these are equivalent:

```bash
git push -u origin main
```

and:

```bash
git push --set-upstream origin main
```

---

# 6. Why Do We Need Upstream?

Imagine you create a new branch:

```bash
git switch -c login-automation
```

This creates the branch locally:

```text
Local:

main
login-automation
```

But GitHub doesn't automatically have this branch yet.

You can publish it with:

```bash
git push -u origin login-automation
```

Now:

```text
Local branch
login-automation
       │
       ↓
Remote branch
origin/login-automation
```

Git now knows that:

```text
login-automation
        ↓
origin/login-automation
```

So future pushes can simply be:

```bash
git push
```

---

# 7. `origin` vs `origin/main`

These are slightly different concepts.

### `origin`

The name of the remote repository.

```text
origin
```

### `origin/main`

The remote-tracking branch representing the `main` branch on that remote.

```text
origin/main
```

Example:

```text
origin
 ├── main
 ├── development
 └── feature-login
```

You will commonly see:

```bash
origin/main
```

when working with branches.

---

# 8. What is `git pull`?

`git pull` gets changes from a remote repository and integrates them into your current local branch.

Basic flow:

```text
GitHub
   ↓
git pull
   ↓
Local Repository
```

Example:

```bash
git pull
```

If your current branch tracks `origin/main`, Git knows where to pull from.

---

# 9. What Actually Happens During `git pull`?

A useful mental model is:

```text
git pull
   =
git fetch
   +
git merge
```

Conceptually:

```text
Remote Repository
       ↓
   git fetch
       ↓
Remote-tracking branch
       ↓
   git merge
       ↓
Current local branch
```

So `git pull` is essentially a convenient combination of fetching remote changes and integrating them into your current branch.

---

# 10. `git fetch` vs `git pull`

### `git fetch`

Downloads information about changes from the remote but does **not automatically merge those changes into your current branch**.

```bash
git fetch
```

Think:

> "Show me what's changed on the remote."

### `git pull`

Fetches the changes and integrates them into your current branch.

```bash
git pull
```

Think:

> "Get the remote changes and update my current branch."

---

# 11. Typical Developer Workflow

Suppose another developer pushed changes to GitHub.

Before starting your work, you might do:

```bash
git pull
```

Then make your changes:

```text
Write code
    ↓
Test code
```

Check:

```bash
git status
```

Stage:

```bash
git add .
```

Commit:

```bash
git commit -m "Added login automation"
```

Push:

```bash
git push
```

Complete flow:

```text
              GitHub
                ↑
                │
              push
                │
        Local Repository
                ↑
              commit
                ↑
              add
                ↑
        Working Directory
                ↑
              pull
                │
              GitHub
```

---

# 12. `git push` vs `git pull`

The easiest way to remember:

```text
git push
    ↓
Local → Remote

git pull
    ↓
Remote → Local
```

Example:

```text
             GitHub
           ↙       ↖
       pull         push
         ↓           ↑
       Local       Local
```

---

# 13. `git push origin main`

You can explicitly tell Git where to push:

```bash
git push origin main
```

Meaning:

> Push my local `main` branch to the `origin` remote.

If the branch has already been configured with upstream tracking, you can usually use:

```bash
git push
```

---

# 14. `git pull origin main`

You can also explicitly specify where to pull from:

```bash
git pull origin main
```

Meaning:

> Pull changes from the `main` branch of the `origin` remote into my current branch.

If upstream tracking is already configured, you can usually use:

```bash
git pull
```

---

# 15. Setting Upstream for a New Branch

Create a branch:

```bash
git switch -c feature-login
```

Push it for the first time:

```bash
git push -u origin feature-login
```

After this:

```bash
git push
```

will normally work without specifying the remote and branch.

You can check the tracking relationship with:

```bash
git branch -vv
```

Example:

```text
* feature-login  abc1234 [origin/feature-login] Added login test
  main           xyz5678 [origin/main] Initial setup
```

The part:

```text
[origin/feature-login]
```

shows the upstream/tracking branch.

---

# 16. `git branch -vv`

This command is useful for seeing which local branches are tracking which remote branches.

```bash
git branch -vv
```

Example:

```text
* main           abc1234 [origin/main] Initial SDET setup
  feature-login  def5678 [origin/feature-login] Added login test
```

This tells us:

```text
main
 ↓
origin/main
```

and:

```text
feature-login
 ↓
origin/feature-login
```

---

# 17. `git remote -v`

Use this when you want to see where your repository is connected.

```bash
git remote -v
```

Example:

```text
origin  git@github.com:username/SDET.git (fetch)
origin  git@github.com:username/SDET.git (push)
```

---

# 18. Changing the Remote URL

If your repository currently uses HTTPS:

```text
https://github.com/username/SDET.git
```

and you want to use SSH:

```bash
git remote set-url origin git@github.com:username/SDET.git
```

Check:

```bash
git remote -v
```

---

# 19. What Happens When You Clone?

When you run:

```bash
git clone git@github.com:username/SDET.git
```

Git automatically:

1. Downloads the repository.
2. Creates a local Git repository.
3. Creates the `origin` remote.
4. Checks out the default branch.
5. Sets up the initial remote-tracking relationship.

So after cloning, you normally don't need to manually run:

```bash
git remote add origin
```

because Git has already configured `origin`.

---

# 20. Common SDET Team Workflow

Imagine you're working on an automation framework with a team.

At the beginning of the day:

```bash
git switch main
git pull
```

Create your feature branch:

```bash
git switch -c feature-login-test
```

Write your automation code.

Check changes:

```bash
git status
```

Stage:

```bash
git add .
```

Commit:

```bash
git commit -m "Added login automation tests"
```

Publish the branch:

```bash
git push -u origin feature-login-test
```

Then the branch can be used for a Pull Request on GitHub.

---

# 21. Important Difference: `git pull` vs Pull Request

These are **not the same thing**.

### `git pull`

A Git command:

```bash
git pull
```

It downloads and integrates remote changes into your local branch.

### Pull Request (PR)

A GitHub collaboration feature.

It is used to propose merging changes from one branch into another.

For example:

```text
feature-login-test
        ↓
   Pull Request
        ↓
      main
```

A PR is a code-review workflow.

---

# 22. Most Important Commands

### Check remote

```bash
git remote -v
```

### Push existing tracked branch

```bash
git push
```

### First push / set upstream

```bash
git push -u origin main
```

### Pull changes

```bash
git pull
```

### Explicit push

```bash
git push origin main
```

### Explicit pull

```bash
git pull origin main
```

### Create and switch to a branch

```bash
git switch -c feature-login
```

### Publish new branch

```bash
git push -u origin feature-login
```

### Check tracking branches

```bash
git branch -vv
```

### Download remote information without merging

```bash
git fetch
```

---

# Quick Mental Model

Remember these four words:

```text
origin
upstream
push
pull
```

### `origin`

> Where is my remote repository?

```text
origin → GitHub repository
```

### `upstream`

> Which remote branch does my local branch track?

```text
main → origin/main
```

### `push`

> Send my commits to the remote.

```text
Local → GitHub
```

### `pull`

> Bring remote changes into my local branch.

```text
GitHub → Local
```

---

# The Workflow to Memorize

For a normal day of development:

```bash
git pull

# Make changes

git status

git add .

git commit -m "Meaningful commit message"

git push
```

For a new feature:

```bash
git switch -c feature-login

# Make changes

git add .

git commit -m "Added login automation"

git push -u origin feature-login
```

After the first push:

```bash
git push
```

is usually enough.

---

# One-Line Interview Answers

**What is `origin`?**

> `origin` is the conventional name given to the remote repository from which a repository was cloned or to which it was connected.

**What is upstream?**

> An upstream branch is the remote branch that a local branch tracks, allowing commands like `git push` and `git pull` to work without explicitly specifying the remote and branch.

**What does `git push -u origin main` do?**

> It pushes the local `main` branch to `origin` and sets `origin/main` as its upstream tracking branch.

**What is the difference between push and pull?**

> `git push` sends local commits to the remote repository, while `git pull` retrieves remote changes and integrates them into the current local branch.

**What is the difference between fetch and pull?**

> `git fetch` downloads remote changes without integrating them into the current branch, while `git pull` fetches and then integrates those changes.

**What is the difference between a Pull Request and `git pull`?**

> A Pull Request is a GitHub code-review and merge workflow, while `git pull` is a Git command used to retrieve and integrate remote changes into a local branch.
