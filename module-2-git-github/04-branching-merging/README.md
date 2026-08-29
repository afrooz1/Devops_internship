# Task 4 — Branching and Merging

## Objective

Practice creating and working with Git branches, making changes independently in a feature branch, switching between branches, merging a feature branch into the `main` branch, checking for merge conflicts, and deleting the feature branch after a successful merge.

The main Git concepts practiced in this task were:

* Creating branches
* Switching branches
* Making changes in an isolated branch
* Committing feature changes
* Merging branches
* Understanding fast-forward merges
* Checking for merge conflicts
* Deleting merged branches
* Viewing branch and commit history

---

## 1. Create a New Repository Directory

I created a new directory for this task:

```bash
mkdir 04-branching-merging
cd 04-branching-merging
```

I verified the current directory:

```bash
pwd
```

I also checked the directory contents:

```bash
ls
```

The directory was initially empty.

---

## 2. Initialize the Git Repository

I initialized the directory as a Git repository:

```bash
git init
```

I checked the repository status:

```bash
git status
```

Git confirmed that the repository was initialized and that there were no commits yet.

---

## 3. Rename the Default Branch to `main`

The repository initially used the default branch name `master`.

I renamed the current branch to `main`:

```bash
git branch -m main
```

I verified the branch:

```bash
git branch
```

Output:

```text
* main
```

---

## 4. Create the Initial Application File

I created an `app.txt` file using Nano:

```bash
nano app.txt
```

I added:

```text
Application: Inventory System
Version: 1.0
Status: Development
```

I checked the repository status:

```bash
git status
```

Git showed `app.txt` as an untracked file.

---

## 5. Stage and Commit the Initial File

I staged the file:

```bash
git add app.txt
```

Then created the initial commit:

```bash
git commit -m "Add initial application file"
```

The initial commit was:

```text
faa0cc9 Add initial application file
```

I checked the commit history:

```bash
git log --oneline
```

Output:

```text
faa0cc9 (HEAD -> main) Add initial application file
```

---

# Part B — Creating and Working on a Feature Branch

## 6. Create the `feature-branch`

I created and switched to a new feature branch:

```bash
git checkout -b feature-branch
```

Git displayed:

```text
Switched to a new branch 'feature-branch'
```

I verified the branches:

```bash
git branch
```

Output:

```text
* feature-branch
  main
```

The `*` indicates the currently active branch.

Initially, both branches pointed to the same commit:

```text
main
  |
  | faa0cc9
  |
  +---- feature-branch
```

---

## 7. Make Changes in the Feature Branch

While working on `feature-branch`, I modified `app.txt`:

```bash
nano app.txt
```

I added:

```text
Feature: Inventory Dashboard
```

The updated file became:

```text
Application: Inventory System
Version: 1.0
Status: Development
Feature: Inventory Dashboard
```

I checked the repository status:

```bash
git status
```

Git showed that `app.txt` had been modified.

---

## 8. Inspect the Feature Changes

I used:

```bash
git diff
```

The output showed the new feature line:

```diff
+Feature: Inventory Dashboard
```

This confirmed that the inventory dashboard change had been made on the feature branch.

---

## 9. Stage the Feature Changes

I staged the modified file:

```bash
git add app.txt
```

I verified the staged changes:

```bash
git status
```

I also used:

```bash
git diff --cached
```

This confirmed that the inventory dashboard change was staged and ready to commit.

---

## 10. Commit the Feature

I committed the feature:

```bash
git commit -m "Add inventory dashboard feature"
```

The feature commit was:

```text
9d630c5 Add inventory dashboard feature
```

I checked the commit history:

```bash
git log --oneline
```

The history showed:

```text
9d630c5 (HEAD -> feature-branch) Add inventory dashboard feature
faa0cc9 (main) Add initial application file
```

This demonstrated that `feature-branch` contained one additional commit that `main` did not yet contain.

The branch structure was:

```text
9d630c5 feature-branch
    |
    |
faa0cc9 main
```

---

# Part C — Switching Back to Main

## 11. Switch to the `main` Branch

I switched back to `main`:

```bash
git checkout main
```

I verified the branches:

```bash
git branch
```

Output:

```text
  feature-branch
* main
```

The `*` indicates that I was now working on `main`.

---

## 12. Verify the Feature Is Not Yet on `main`

I checked the contents of `app.txt`:

```bash
cat app.txt
```

Output:

```text
Application: Inventory System
Version: 1.0
Status: Development
```

The following line was not present:

```text
Feature: Inventory Dashboard
```

This confirmed that the feature had not yet been merged into `main`.

I also checked:

```bash
git status
```

The working tree was clean.

---

# Part D — Merging the Feature Branch

## 13. Merge `feature-branch` into `main`

I merged the feature branch:

```bash
git merge feature-branch
```

Git displayed:

```text
Updating faa0cc9..9d630c5
Fast-forward
 app.txt | 1 +
 1 file changed, 1 insertion(+)
```

The merge was completed using a **fast-forward merge**.

No merge conflict occurred.

---

## 14. Understand the Fast-Forward Merge

Before the merge:

```text
9d630c5 feature-branch
    |
    |
faa0cc9 main
```

Because `main` had not received any new commits after the feature branch was created, Git simply moved the `main` branch pointer forward.

After the merge:

```text
9d630c5 main, feature-branch
    |
    |
faa0cc9
```

Both branches pointed to the same commit.

A fast-forward merge does not create a separate merge commit because there was no divergent history to combine.

---

## 15. Verify the Merged Feature

I checked the contents of `app.txt`:

```bash
cat app.txt
```

Output:

```text
Application: Inventory System
Version: 1.0
Status: Development
Feature: Inventory Dashboard
```

This confirmed that the feature had successfully been merged into `main`.

---

## 16. Check Repository Status After the Merge

I ran:

```bash
git status
```

The result confirmed:

```text
On branch main
nothing to commit, working tree clean
```

This confirmed that the merge completed successfully and there were no uncommitted changes.

---

## 17. Verify the Commit History

I used:

```bash
git log --oneline
```

The history showed:

```text
9d630c5 (HEAD -> main, feature-branch) Add inventory dashboard feature
faa0cc9 Add initial application file
```

This confirmed that the feature commit was now part of `main`.

---

# Part E — Merge Conflict Handling

## 18. Check for Merge Conflicts

A merge conflict can occur when different branches modify the same part of a file in incompatible ways.

In this task, no merge conflict occurred because `main` had not been modified after the feature branch was created.

Git performed a fast-forward merge:

```text
Updating faa0cc9..9d630c5
Fast-forward
```

Therefore, no manual conflict resolution was required.

---

# Part F — Delete the Feature Branch

## 19. Delete the Merged Feature Branch

After successfully merging the feature, I deleted the feature branch:

```bash
git branch -d feature-branch
```

Git displayed:

```text
Deleted branch feature-branch (was 9d630c5).
```

This confirmed that the feature branch was safely deleted after being merged.

---

## 20. Verify the Remaining Branch

I checked the branches:

```bash
git branch
```

Output:

```text
* main
```

This confirmed that `feature-branch` had been deleted and `main` remained as the active branch.

---

# Important Concepts Learned

## `git checkout -b`

Creates a new branch and immediately switches to it.

```bash
git checkout -b feature-branch
```

This allows new features to be developed independently from `main`.

---

## `git checkout`

Switches between existing branches.

```bash
git checkout main
```

---

## Git Branches

Branches allow development work to be isolated.

In this task:

```text
main
 |
 | initial application
 |
 +---- feature-branch
          |
          | inventory dashboard
          |
          v
      feature commit
```

---

## `git merge`

Combines changes from one branch into the currently checked-out branch.

```bash
git checkout main
git merge feature-branch
```

The current branch receives the changes from the specified branch.

---

## Fast-Forward Merge

A fast-forward merge happens when the target branch has no new commits since the feature branch was created.

In this task:

```text
Before merge:

9d630c5 feature-branch
    |
    |
faa0cc9 main
```

After merge:

```text
9d630c5 main, feature-branch
    |
    |
faa0cc9
```

Git simply moved the `main` pointer forward.

---

## Merge Conflict

A merge conflict may occur when two branches make different changes to the same part of a file.

For example:

```text
main:
Version: 2.0

feature-branch:
Version: 3.0
```

Git may require manual intervention to determine which change should be kept.

In this task, no conflict occurred.

---

## `git branch -d`

Deletes a local branch that has already been merged:

```bash
git branch -d feature-branch
```

---

# Commands Practiced

```bash
mkdir 04-branching-merging
cd 04-branching-merging

pwd
ls
git init
git status

git branch -m main
git branch

nano app.txt

git add app.txt
git commit -m "Add initial application file"

git branch
git log --oneline

git checkout -b feature-branch
git branch

nano app.txt

git status
git diff

git add app.txt
git status
git diff --cached

git commit -m "Add inventory dashboard feature"

git log
git checkout main

git branch
cat app.txt
git status

git merge feature-branch

cat app.txt
git status
git log

git branch -d feature-branch
git branch
```

---

# Final Repository State

The final active branch was:

```text
* main
```

The feature branch was deleted after the successful merge:

```text
Deleted branch feature-branch (was 9d630c5).
```

The final `app.txt` contained:

```text
Application: Inventory System
Version: 1.0
Status: Development
Feature: Inventory Dashboard
```

The important commits were:

```text
9d630c5 Add inventory dashboard feature
faa0cc9 Add initial application file
```

---

# Result

The task was successfully completed.

I created a Git repository and configured the primary branch as `main`. I created a separate `feature-branch` using `git checkout -b`, made changes to the application file, staged and committed those changes, and switched back to `main`.

I verified that the feature was not present on `main` before merging. I then merged `feature-branch` into `main` using `git merge`.

Git performed a **fast-forward merge** because `main` had no new commits after the feature branch was created. No merge conflict occurred.

After verifying the merged feature and confirming that the working tree was clean, I deleted the merged feature branch using:

```bash
git branch -d feature-branch
```

The final repository contained only the `main` branch.

This task provided practical experience with:

* Creating branches
* Switching between branches
* Isolating feature development
* Committing changes on a feature branch
* Merging branches
* Understanding fast-forward merges
* Checking for merge conflicts
* Deleting merged branches
* Maintaining a clean Git branching workflow

The final commit history was:

```text
main
 |
 +-- faa0cc9 Add initial application file
 |
 +-- 9d630c5 Add inventory dashboard feature
```

The feature was successfully integrated into `main`, and the temporary feature branch was safely removed.
