# Task 3 — Tracking Changes and Viewing History

## Objective

Practice tracking changes in a Git repository, understanding the difference between the working directory and staging area, viewing file differences, creating multiple commits, inspecting commit history, and identifying which commit last modified each line of a file.

---

## 1. Create a New Repository Directory

I created a new directory for this task:

```bash
mkdir 03-tracking-history
cd 03-tracking-history
```

I initialized the directory as a Git repository:

```bash
git init
```

Git successfully created the repository:

```text
Initialized empty Git repository in /home/afrooz/module-2-git-github/03-tracking-history/.git/
```

I verified the repository status:

```bash
git status
```

Output:

```text
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

This confirmed that the repository had been initialized but did not contain any commits yet.

---

## 2. Create the Initial Application File

I created an `app.txt` file using Nano:

```bash
nano app.txt
```

I added the following content:

```text
Application: Inventory System
Version: 1.0
Status: Development
```

I then checked the repository status:

```bash
git status
```

Git reported:

```text
Untracked files:

        app.txt
```

This meant that `app.txt` existed in the working directory but was not yet being tracked by Git.

---

## 3. Stage the File

I added the file to the staging area:

```bash
git add app.txt
```

I verified the staging state:

```bash
git status
```

Git reported:

```text
Changes to be committed:

        new file:   app.txt
```

This demonstrated that `git add` moves the file from the working directory into the staging area.

The workflow was:

```text
Working Directory
       |
       | git add
       v
Staging Area
```

---

## 4. Create the First Commit

I committed the staged file:

```bash
git commit -m "Add initial application file"
```

Git created the first commit:

```text
[master (root-commit) 9287c0c] Add initial application file
 1 file changed, 3 insertions(+)
 create mode 100644 app.txt
```

The commit hash was:

```text
9287c0c
```

The commit message was:

```text
Add initial application file
```

Because this was the first commit, Git identified it as the `root-commit`.

---

## 5. Verify the Working Tree

I checked the repository status:

```bash
git status
```

Output:

```text
On branch master
nothing to commit, working tree clean
```

This confirmed that the working directory contained no changes that needed to be committed.

---

# Part B — Tracking Modifications

## 6. Modify the Application File

I opened `app.txt` again:

```bash
nano app.txt
```

I changed:

```text
Version: 1.0
```

to:

```text
Version: 1.1
```

I also added:

```text
Environment: Development
```

The updated file became:

```text
Application: Inventory System
Version: 1.1
Status: Development
Environment: Development
```

---

## 7. Check the Modified File with `git status`

I ran:

```bash
git status
```

Git reported:

```text
Changes not staged for commit:

        modified:   app.txt
```

This showed that Git detected changes to a previously committed file, but those changes had not yet been staged.

The state was:

```text
Last Commit
     |
     | file modified
     v
Working Directory
```

---

## 8. View Changes with `git diff`

I used:

```bash
git diff
```

Git displayed:

```diff
diff --git a/app.txt b/app.txt
index dcba597..f41ce4e 100644
--- a/app.txt
+++ b/app.txt
@@ -1,3 +1,4 @@
 Application: Inventory System
-Version: 1.0
+Version: 1.1
 Status: Development
+Environment: Development
```

This demonstrated that `git diff` shows changes between the last committed version and the current unstaged working directory.

The changes were:

```diff
-Version: 1.0
+Version: 1.1
+Environment: Development
```

---

## 9. Stage the Modified File

I staged the changes:

```bash
git add app.txt
```

I checked the status:

```bash
git status
```

Git reported:

```text
Changes to be committed:

        modified:   app.txt
```

This confirmed that the changes had moved into the staging area.

---

## 10. Understand `git diff` After Staging

I ran:

```bash
git diff
```

No output was displayed.

This happened because there were no longer any unstaged changes.

The working directory and staging area now contained the same version.

To view the staged changes, I could use:

```bash
git diff --cached
```

This compares:

```text
Staging Area
      |
      | compare
      v
Last Commit
```

The important difference is:

```text
git diff
```

shows **unstaged changes**.

```text
git diff --cached
```

shows **staged changes**.

---

## 11. Create the Second Commit

I committed the staged changes:

```bash
git commit -m "Update application version and environment"
```

Git created the second commit:

```text
[master 6196cfd] Update application version and environment
 1 file changed, 2 insertions(+), 1 deletion(-)
```

The commit hash was:

```text
6196cfd
```

I verified the repository status:

```bash
git status
```

Output:

```text
On branch master
nothing to commit, working tree clean
```

---

# Part C — Viewing Git History

## 12. View Detailed Commit History

I used:

```bash
git log
```

Git displayed the repository history:

```text
commit 6196cfd699a82d13c6158e8ef17d6debca91b865 (HEAD -> master)
Author: afrooz1 <afroozhabib2@gmail.com>
Date:   Wed Aug 19 12:58:46 2026 +0500

    Update application version and environment

commit 9287c0c01d4f237cd7b768862050a8c3633e5142
Author: afrooz1 <afroozhabib2@gmail.com>
Date:   Wed Aug 19 12:51:47 2026 +0500

    Add initial application file
```

This showed that the repository now contained two commits.

`git log` provides detailed information including:

* Commit hash
* Author
* Date
* Commit message
* Branch reference

---

# Part D — Creating a Third Commit

## 13. Make Another Modification

I modified `app.txt` again:

```bash
nano app.txt
```

I changed:

```text
Status: Development
```

to:

```text
Status: Testing
```

I then checked the repository:

```bash
git status
```

Git reported:

```text
Changes not staged for commit:

        modified:   app.txt
```

---

## 14. View the Third Change with `git diff`

I ran:

```bash
git diff
```

Git showed the change:

```diff
-Status: Development
+Status: Testing
```

The diff also showed that a blank line had been added at the beginning of the file during this edit.

---

## 15. Stage and Commit the Third Change

I staged the file:

```bash
git add app.txt
```

Then created the third commit:

```bash
git commit -m "Update application status to testing"
```

Git created:

```text
[master 19fd7c2] Update application status to testing
 1 file changed, 2 insertions(+), 1 deletion(-)
```

The commit hash was:

```text
19fd7c2
```

---

## 16. View Compact Commit History

I used:

```bash
git log --oneline
```

The final history was:

```text
19fd7c2 (HEAD -> master) Update application status to testing
6196cfd Update application version and environment
9287c0c Add initial application file
```

This provided a compact view of all three commits.

### Difference between the commands

```bash
git log
```

Displays detailed commit history.

```bash
git log --oneline
```

Displays a compact one-line history.

---

# Part E — Using `git blame`

## 17. Identify Line-Level Changes

I used:

```bash
git blame app.txt
```

The output was:

```text
19fd7c2b (afrooz1 2026-08-19 13:03:04 +0500 1)
^9287c0c (afrooz1 2026-08-19 12:51:47 +0500 2) Application: Inventory System
6196cfd6 (afrooz1 2026-08-19 12:58:46 +0500 3) Version: 1.1
19fd7c2b (afrooz1 2026-08-19 13:03:04 +0500 4) Status: Testing
6196cfd6 (afrooz1 2026-08-19 12:58:46 +0500 5) Environment: Development
```

This demonstrated that `git blame` can identify which commit last modified each line.

For example:

```text
Application: Inventory System
```

was originally introduced in:

```text
9287c0c
```

while:

```text
Version: 1.1
Environment: Development
```

were introduced or modified in:

```text
6196cfd
```

and:

```text
Status: Testing
```

was changed in:

```text
19fd7c2
```

---

# Important Concepts Learned

### `git status`

Shows the current state of the working directory and staging area.

```bash
git status
```

It can show:

* Untracked files
* Modified files
* Staged files
* Clean working tree

---

### `git diff`

Shows changes that have not yet been staged.

```bash
git diff
```

The comparison is:

```text
Last Commit
     |
     | compare
     v
Working Directory
```

---

### `git add`

Moves changes from the working directory into the staging area.

```bash
git add app.txt
```

Workflow:

```text
Working Directory
       |
       | git add
       v
Staging Area
```

---

### `git diff --cached`

Shows changes currently staged for the next commit.

```bash
git diff --cached
```

Workflow:

```text
Last Commit
     |
     | compare
     v
Staging Area
```

---

### `git commit`

Creates a permanent snapshot of the staged changes.

```bash
git commit -m "Commit message"
```

---

### `git log`

Displays detailed commit history.

```bash
git log
```

---

### `git log --oneline`

Displays a compact version of the commit history.

```bash
git log --oneline
```

---

### `git blame`

Shows which commit and author last modified each line of a file.

```bash
git blame app.txt
```

This can be useful when investigating when or why a particular line was introduced.

---

# Git Change Tracking Workflow

The main workflow practiced in this task was:

```text
Edit/Create File
       |
       v
Working Directory
       |
       | git status
       | git diff
       |
       | git add
       v
Staging Area
       |
       | git diff --cached
       |
       | git commit
       v
Git Repository
       |
       | git log
       | git log --oneline
       | git blame
       v
Commit History
```

---

# Commands Practiced

```bash
mkdir 03-tracking-history
cd 03-tracking-history

git init
git status

nano app.txt

git status
git add app.txt
git status

git commit -m "Add initial application file"
git status

nano app.txt

git status
git diff

git add app.txt
git status
git diff
git diff --cached

git commit -m "Update application version and environment"
git status

git log

nano app.txt

git status
git diff

git add app.txt

git commit -m "Update application status to testing"

git log --oneline
git blame app.txt
```

---

## Result

The task was successfully completed.

I created and initialized a Git repository, created and committed an application file, modified the tracked file, inspected unstaged changes using `git diff`, staged the changes using `git add`, inspected staged changes using `git diff --cached`, and created additional commits.

I also viewed detailed and compact commit history using `git log` and `git log --oneline`, and used `git blame` to identify which commits were responsible for individual lines in the file.

The practical exercise demonstrated the difference between:

* Checking repository state with `git status`
* Viewing unstaged changes with `git diff`
* Staging changes with `git add`
* Viewing staged changes with `git diff --cached`
* Saving snapshots with `git commit`
* Viewing detailed history with `git log`
* Viewing compact history with `git log --oneline`
* Identifying line-level history with `git blame`

The final repository contained three commits:

```text
19fd7c2 Update application status to testing
6196cfd Update application version and environment
9287c0c Add initial application file
```

This task provided practical understanding of how Git tracks changes from the working directory, through the staging area, and finally into the repository history.
