# Task 6 — Reverting and Resetting Commits

## Objective

Practice different Git techniques for undoing commits and restoring files.

In this task, I practiced:

* Using `git reset --soft HEAD~1` to undo the latest commit while keeping its changes staged.
* Using `git reset --hard HEAD~1` to remove the latest commit and discard its changes.
* Using `git revert` to undo a previous commit by creating a new commit without removing the original commit from history.
* Using `git checkout <commit> -- <file>` to restore a file to the version from a specific commit.
* Understanding the differences between reset, revert, and checkout.

---

## 1. Create Repository

Created a new directory for Task 6 and initialized it as a Git repository.

```bash
mkdir 06-reverting-resetting
cd 06-reverting-resetting
git init
```

Git initialized the repository using the `master` branch.

---

## 2. Create Initial Commit

Created `app.txt` with the initial application version.

```text
Version 1 of application
```

Then staged and committed the file:

```bash
git add app.txt
git commit -m "Add initial application version"
```

Initial commit:

```text
60408fa Add initial application version
```

---

## 3. Create Second Commit

Modified `app.txt` and added a user login feature.

```text
Version 1 of application
Feature: User Login
```

Then created a second commit:

```bash
git add app.txt
git commit -m "Add user login feature"
```

The history contained:

```text
3705fa4 Add user login feature
60408fa Add initial application version
```

---

## 4. Practice `git reset --soft HEAD~1`

Used:

```bash
git reset --soft HEAD~1
```

This removed the latest commit from the current branch history but kept the changes from that commit staged.

Verified using:

```bash
git status
```

The changes appeared as staged modifications:

```text
Changes to be committed:
        modified: app.txt
```

The changes were then committed again with a new commit message:

```bash
git commit -m "Add user login feature after fix"
```

### Key Learning

`git reset --soft HEAD~1` is useful when I want to undo a commit but keep the changes staged so I can modify or recommit them.

---

## 5. Practice `git reset --hard HEAD~1`

Next, practiced the hard reset:

```bash
git reset --hard HEAD~1
```

Git moved `HEAD` back to the previous commit:

```text
60408fa Add initial application version
```

The changes from the removed commit were also discarded from the working directory.

Verified the result using:

```bash
git log --oneline
git status
cat app.txt
```

The file contained only:

```text
Version 1 of application
```

The working tree was clean:

```text
nothing to commit, working tree clean
```

### Key Learning

`git reset --hard HEAD~1` removes the latest commit and resets the working directory to the previous commit.

It should be used carefully because uncommitted changes can be permanently discarded.

---

## 6. Practice `git revert`

Created the user login feature again and committed it:

```bash
git add app.txt
git commit -m "Add user login feature"
```

The history became:

```text
3705fa4 Add user login feature
60408fa Add initial application version
```

Then reverted the latest commit:

```bash
git revert HEAD
```

Git created a new commit:

```text
0979bb9 Revert "Add user login feature"
```

The final history became:

```text
0979bb9 Revert "Add user login feature"
3705fa4 Add user login feature
60408fa Add initial application version
```

The original `Add user login feature` commit remained in the history.

The file was restored to:

```text
Version 1 of application
```

### Key Learning

`git revert` does not remove the original commit. Instead, it creates a new commit that reverses the changes introduced by that commit.

This makes `git revert` safer when working with commits that have already been pushed to a shared repository.

---

## 7. Practice `git checkout` with a Specific Commit

After reverting the user login feature, the file contained:

```text
Version 1 of application
```

Used:

```bash
git checkout 3705fa4 -- app.txt
```

This restored `app.txt` to the version that existed in commit `3705fa4`.

The file then contained:

```text
Version 1 of application
Feature: User Login
```

The commit history remained unchanged:

```text
0979bb9 Revert "Add user login feature"
3705fa4 Add user login feature
60408fa Add initial application version
```

### Key Learning

```bash
git checkout <commit> -- <file>
```

can restore a specific file from an older commit without moving `HEAD` or deleting commit history.

---

## 8. Important Git Commands

| Command                           | Purpose                                              |
| --------------------------------- | ---------------------------------------------------- |
| `git reset --soft HEAD~1`         | Removes the latest commit but keeps changes staged   |
| `git reset --hard HEAD~1`         | Removes the latest commit and discards its changes   |
| `git revert HEAD`                 | Creates a new commit that reverses the latest commit |
| `git checkout <commit> -- <file>` | Restores a file from a specific commit               |
| `git log --oneline`               | Displays compact commit history                      |
| `git status`                      | Shows the current repository state                   |
| `cat app.txt`                     | Displays the contents of the file                    |

---

## 9. Reset vs Revert

### Reset

Reset changes the existing commit history.

Before:

```text
A → B → C
```

After reset:

```text
A → B
```

The branch pointer is moved backward.

---

### Revert

Revert preserves the existing history and creates a new commit.

Before:

```text
A → B → C
```

After revert:

```text
A → B → C → D
```

Commit `D` contains the changes that undo commit `C`.

The original commit `C` remains in the history.

---

## 10. Soft Reset vs Hard Reset

| Feature                               | Soft Reset | Hard Reset |
| ------------------------------------- | ---------- | ---------- |
| Removes commit from current history   | Yes        | Yes        |
| Keeps changes                         | Yes        | No         |
| Changes remain staged                 | Yes        | No         |
| Can discard working-directory changes | No         | Yes        |
| Risk level                            | Lower      | Higher     |

### Simple Rule

```text
--soft → Remove commit, keep my work

--hard → Remove commit, discard my work
```

---

## 11. When to Use Each Command

### Use `git reset --soft`

When I made a commit too early or want to change the commit message while keeping the changes staged.

### Use `git reset --hard`

When I want to completely discard a commit and its changes and I am sure the work is no longer needed.

### Use `git revert`

When a commit has already been pushed or shared and I want to undo its changes while preserving the project history.

### Use `git checkout <commit> -- <file>`

When I only need to restore a particular file from an earlier commit.

---

## 12. Final Git History

The final repository history after the revert was:

```text
0979bb9 (HEAD -> master) Revert "Add user login feature"
3705fa4 Add user login feature
60408fa Add initial application version
```

This demonstrated that `git revert` preserves the original commit while adding a new commit that reverses its changes.

---

## What I Learned

Through this task, I learned that Git provides multiple ways to undo or restore work, and each method has a different purpose.

I learned that:

* `git reset --soft` removes a commit while keeping the changes staged.
* `git reset --hard` removes a commit and discards its changes.
* `git revert` creates a new commit that reverses an existing commit without deleting it from history.
* `git checkout <commit> -- <file>` can restore an individual file from a specific commit.
* Reset changes existing history, while revert preserves history.
* `git revert` is generally safer for commits that have already been pushed or shared.
* `git reset --hard` should be used carefully because it can discard work.

This task improved my understanding of Git history management and gave me practical experience handling mistakes, undoing commits, and restoring previous versions of files.

---

## Task 6 Status

*  `git reset --soft HEAD~1`
*  `git reset --hard HEAD~1`
*  `git revert HEAD`
*  `git checkout <commit> -- <file>`
