# Task 2 — Initializing and Cloning Repositories

## Objective

Practice creating a new Git repository, creating and committing a file, cloning an existing public repository, and exploring the `.git` directory and remote repository configuration.

---

## 1. Create a New Repository Directory

I created a new directory for this task:

```bash
mkdir 02-init-and-clone
cd 02-init-and-clone
```

I verified the directory was initially empty using:

```bash
ls -la
```

---

## 2. Initialize the Git Repository

I initialized the directory as a Git repository:

```bash
git init
```

Git created the repository successfully:

```text
Initialized empty Git repository in /home/afrooz/module-2-git-github/02-init-and-clone/.git/
```

I verified the hidden `.git` directory:

```bash
ls -la
```

The directory contained:

```text
.git
```

This confirmed that Git had initialized the directory and created the internal repository structure.

---

## 3. Create a README File

I created and edited a `README.md` file using Nano:

```bash
nano README.md
```

I added sample content to the file.

I then checked the repository status:

```bash
git status
```

Git reported:

```text
Untracked files:
        README.md
```

This meant the file existed in the working directory but was not yet being tracked by Git.

---

## 4. Stage the File

I added the README file to the staging area:

```bash
git add README.md
```

I verified the staging state:

```bash
git status
```

Git reported:

```text
Changes to be committed:

        new file:   README.md
```

This demonstrated that `git add` moves a file from the working directory into the staging area.

The workflow was:

```text
Working Directory
       |
       | git add
       v
Staging Area
```

---

## 5. Create the First Commit

I committed the staged file:

```bash
git commit -m "Add initial README"
```

Git created the first commit:

```text
[master (root-commit) f421500] Add initial README
 1 file changed, 3 insertions(+)
 create mode 100644 README.md
```

The commit hash was:

```text
f421500
```

The commit message was:

```text
Add initial README
```

This was the repository's first commit, therefore Git identified it as the `root-commit`.

---

## 6. Verify the Working Tree

I checked the repository status:

```bash
git status
```

Output:

```text
On branch master
nothing to commit, working tree clean
```

This confirmed that all current changes had been committed.

---

## 7. View Commit History

I viewed the commit history using:

```bash
git log --oneline
```

Output:

```text
f421500 (HEAD -> master) Add initial README
```

This showed the commit hash, current branch reference, and commit message.

---

# Part B — Clone an Existing Repository

## 8. Clone a Public GitHub Repository

I moved back to the Git/GitHub module directory:

```bash
cd ~/module-2-git-github
```

I cloned the public `Hello-World` repository:

```bash
git clone https://github.com/octocat/Hello-World.git
```

Git successfully downloaded the repository:

```text
Cloning into 'Hello-World'...
remote: Enumerating objects: 13, done.
remote: Total 13 (delta 0), reused 0 (delta 0), pack-reused 13
Receiving objects: 100% (13/13), done.
```

A new directory named `Hello-World` was created.

---

## 9. Explore the Cloned Repository

I listed the contents:

```bash
ls Hello-World
```

The repository contained:

```text
README
```

I entered the cloned repository:

```bash
cd Hello-World
```

Then I inspected all files, including hidden files:

```bash
ls -la
```

The output showed:

```text
.git
README
```

This confirmed that the cloned repository contained its own `.git` directory.

---

## 10. Verify the Cloned Repository

I ran:

```bash
git status
```

Git reported:

```text
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```

This confirmed that the repository was successfully cloned and connected to its remote repository.

---

## 11. View the Cloned Repository History

I initially tried:

```bash
git log --oneline 5
```

This produced an error because Git interpreted `5` as a revision.

The correct command was:

```bash
git log --oneline -5
```

This displayed the latest available commits:

```text
7fd1a60 (HEAD -> master, origin/master, origin/HEAD) Merge pull request #6 from Spaceghost/patch-1
7629413 New line at end of file. --Signed off by Spaceghost
553c207 first commit
```

This demonstrated that cloning a repository also downloads its existing Git history.

---

## 12. Check the Remote Repository

I checked the remote configuration:

```bash
git remote -v
```

Output:

```text
origin  https://github.com/octocat/Hello-World.git (fetch)
origin  https://github.com/octocat/Hello-World.git (push)
```

This showed that the remote repository was named:

```text
origin
```

and pointed to:

```text
https://github.com/octocat/Hello-World.git
```

The remote has separate fetch and push URLs, which were currently the same.

---

## Important Concepts Learned

### `git init`

Creates a new Git repository in an existing directory.

```text
Existing folder
      |
      | git init
      v
Git repository
```

### `git clone`

Creates a local copy of an existing remote repository, including its files and Git history.

```text
Remote repository
       |
       | git clone
       v
Local repository
       +
     .git
       +
   Git history
```

### `git add`

Moves changes into the staging area.

### `git commit`

Creates a permanent snapshot of the staged changes in Git history.

### `.git`

Contains Git's internal repository information and history.

### `origin`

The conventional default name Git gives to the remote repository when it is cloned.

---

## Git Workflow Practiced

```text
Create directory
       |
       | git init
       v
Git repository
       |
       | Create README.md
       v
Working Directory
       |
       | git add
       v
Staging Area
       |
       | git commit
       v
Repository History
```

For the second part:

```text
GitHub Repository
       |
       | git clone
       v
Local Repository
       |
       +── Project files
       |
       +── .git
       |
       +── Commit history
       |
       +── origin remote
```

---

## Commands Practiced

```bash
mkdir 02-init-and-clone
cd 02-init-and-clone
git init
ls -la
nano README.md
git status
git add README.md
git commit -m "Add initial README"
git status
git log --oneline

cd ..
git clone https://github.com/octocat/Hello-World.git
ls Hello-World
cd Hello-World
ls -la
git status
git log --oneline -5
git remote -v
```

---

## Result

The task was successfully completed.

I created and initialized a local Git repository, created and committed a README file, cloned a public GitHub repository, explored its structure, identified the `.git` directory, inspected its commit history, and verified its remote configuration.

The practical exercise demonstrated the difference between:

* Creating a new repository with `git init`
* Copying an existing repository with `git clone`
* Tracking files with `git add`
* Saving snapshots with `git commit`
* Viewing history with `git log`
* Viewing remote configuration with `git remote -v`
