# Task 1 — Basic Git Setup

## Objective

Set up Git on the Ubuntu WSL environment, configure the Git user identity, create a Git repository, inspect the repository's internal `.git` directory, and create a `.gitignore` file.

---

## 1. Verify Git Installation

First, I checked whether Git was installed:

```bash
git --version
```

Output:

```text
git version 2.43.0
```

This confirmed that Git was already installed on the Ubuntu WSL environment.

---

## 2. Configure Git Username and Email

I configured my Git identity globally using:

```bash
git config --global user.name ""
git config --global user.email ""
```

The `--global` option applies this configuration to Git repositories for my user account on this system.

I verified the configuration with:

```bash
git config --list
```

Output:

```text
user.name=
user.email=
```

### Important Note

Git uses this name and email to identify the author of commits.

This configuration is different from GitHub authentication. It identifies who created a commit; it does not log the user into GitHub.

---

## 3. Create the Git Practice Directory

I created a directory for the Git and GitHub module:

```bash
mkdir -p module-2-git-github
cd module-2-git-github
```

I verified the current location:

```bash
pwd
```

Output:

```text
/home/afrooz/module-2-git-github
```

---

## 4. Create the Git Practice Repository

Inside the module directory, I created a project directory:

```bash
mkdir git-basics
cd git-basics
```

Before initializing Git, the directory contained no project files.

---

## 5. Initialize the Git Repository

I initialized the directory as a Git repository:

```bash
git init
```

Git returned:

```text
Initialized empty Git repository in /home/afrooz/module-2-git-github/git-basics/.git/
```

Git created a hidden `.git` directory.

I verified it with:

```bash
ls -la
```

Output showed:

```text
.git
```

### Understanding `.git`

The `.git` directory contains Git's internal repository data, including information required to track the repository, branches, references, objects, and configuration.

I inspected it using:

```bash
ls -la .git
```

The directory contained items such as:

```text
HEAD
branches
config
description
hooks
info
objects
refs
```

The `.git` directory is what makes the project directory a Git repository.

---

## 6. Check Repository Status

I checked the repository status:

```bash
git status
```

Output:

```text
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

This showed that:

* The repository was successfully initialized.
* The current branch was `master`.
* No commits existed yet.
* There were no files being tracked yet.

---

## 7. Create `.gitignore`

I created a `.gitignore` file:

```bash
touch .gitignore
```

I verified that it existed:

```bash
ls -la
```

The directory now contained:

```text
.git
.gitignore
```

I then opened the file with Nano:

```bash
nano .gitignore
```

I added:

```text
*.log
.env
node_modules/
```

### Purpose of `.gitignore`

`.gitignore` specifies files and directories that Git should ignore instead of tracking.

For example:

* `*.log` ignores log files.
* `.env` ignores environment files that may contain sensitive configuration.
* `node_modules/` ignores installed Node.js dependencies.

---

## 8. Final Repository Status

After creating `.gitignore`, I ran:

```bash
git status
```

Git reported:

```text
Untracked files:
        .gitignore

nothing added to commit but untracked files present
```

This means Git can see `.gitignore`, but it has not been added to the staging area yet.


At this stage, `.gitignore` is still in the **Working Directory** and is **untracked**.

---

## Key Learnings

1. Git was already installed on the WSL Ubuntu environment.
2. `git config --global` was used to configure the Git username and email.
3. `git init` converted a normal directory into a Git repository.
4. Git creates a hidden `.git` directory to store repository information.
5. `git status` shows the current state of the repository.
6. A newly created file is initially **untracked**.
7. `.gitignore` defines files and directories that should not be tracked by Git.
8. Files must eventually move from the working directory to the staging area using `git add` before they can be committed.

---

## Commands Practiced

```bash
git --version
git config --global user.name "afrooz"
git config --global user.email "addedmin@gmail.com"
git config --list
mkdir -p module-2-git-github
cd module-2-git-github
mkdir git-basics
cd git-basics
git init
ls -la
ls -la .git
git status
touch .gitignore
nano .gitignore
```

---

## Result

The Git environment was successfully configured, and a local Git repository was initialized at:

```text
/home/afrooz/module-2-git-github/git-basics
```

The repository contains a `.git` directory and a `.gitignore` file. The `.gitignore` file is currently untracked and has not yet been committed.
