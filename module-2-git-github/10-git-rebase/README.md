# Git Rebase

## Objective

Practice using **Git Rebase** to update a feature branch with the latest changes from `master`, resolve rebase conflicts, and safely push rewritten history to a remote repository.

In this task, I practiced:

* Creating a feature branch from `master`
* Making multiple commits on a feature branch
* Pushing a feature branch to GitHub
* Making independent commits on `master`
* Rebasing a feature branch onto `master`
* Understanding how rebase rewrites commit history
* Creating and resolving a rebase conflict
* Using `git rebase --continue`
* Understanding `git rebase --abort`
* Using `git push --force-with-lease`
* Visualizing Git history with `git log --oneline --graph`

---

## Repository Setup

Repository:

```text
Git-Rebase
```

Local directory:

```text
~/module-2-git-github/10-git-rebase
```

Remote repository:

```text
https://github.com/afrooz1/Git-Rebase.git
```

---

## 1. Initialize the Repository

Created the project directory and initialized Git:

```bash
mkdir 10-git-rebase
cd 10-git-rebase
git init
```

The repository initially used `master` as the default branch.

Created the initial `README.md` and committed it:

```bash
git add README.md
git commit -m "Initial commit"
```

Initial commit:

```text
ef7a303 Initial commit
```

---

## 2. Create Feature Branch

Created a new branch:

```bash
git checkout -b feature-rebase
```

Made two commits on the feature branch:

```text
3f43c2f Add feature file
3ae15e3 Update feature
```

The feature branch was pushed to GitHub:

```bash
git push -u origin feature-rebase
```

---

## 3. Make Changes on Master

Switched back to `master`:

```bash
git checkout master
```

Created two commits independently on `master`:

```text
36e1291 Add main update
1f127bf Update main branch
```

At this point, the branches had different histories.

---

## 4. Rebase Feature Branch

Switched back to the feature branch:

```bash
git checkout feature-rebase
```

Rebased it onto the latest `master`:

```bash
git rebase master
```

The rebase completed successfully.

The original feature commits:

```text
3f43c2f Add feature file
3ae15e3 Update feature
```

were rewritten as new commits:

```text
a356f81 Add feature file
352970f Update feature
```

This demonstrated that **rebase creates new commit IDs because it rewrites commit history**.

---

## 5. Understanding Remote Divergence

After rebasing, Git reported that the local feature branch and remote feature branch had diverged.

This happened because the feature branch history had been rewritten locally.

Instead of using a normal force push:

```bash
git push --force
```

I used the safer option:

```bash
git push --force-with-lease origin feature-rebase
```

The remote branch was successfully updated.

---

## 6. Practice Rebase Conflict

To practice conflict resolution, I modified `README.md` independently on both branches.

On `master`:

```text
11e763b Update README on main
```

On `feature-rebase`:

```text
6d35880 Update README on feature
```

Then I attempted:

```bash
git rebase master
```

Git detected a conflict:

```text
CONFLICT (content): Merge conflict in README.md
```

---

## 7. Resolve the Conflict

Opened the conflicted file:

```bash
nano README.md
```

Resolved the conflicting content and then staged the resolved file:

```bash
git add README.md
```

Continued the rebase:

```bash
git rebase --continue
```

The rebase completed successfully with the new commit:

```text
9a57426 Update README on feature and main
```

---

## 8. Rebase Conflict Commands

During a rebase conflict, the important commands are:

### Check the current state

```bash
git status
```

### Stage the resolved file

```bash
git add <file>
```

### Continue the rebase

```bash
git rebase --continue
```

### Cancel the rebase

```bash
git rebase --abort
```

### Skip the problematic commit

```bash
git rebase --skip
```

---

## 9. Important Difference Between Main and Master

This repository uses:

```text
master
```

instead of:

```text
main
```

Therefore:

```bash
git rebase main
```

returned:

```text
fatal: invalid upstream 'main'
```

The correct command for this repository was:

```bash
git rebase master
```

This demonstrated that `main` and `master` are branch names, not special Git commands.

---

## 10. Final Push

Because the feature branch history was rewritten by rebase, I used:

```bash
git push --force-with-lease origin feature-rebase
```

The push completed successfully:

```text
feature-rebase -> feature-rebase (forced update)
```

---

## Git Rebase Concept

### Before Rebase

```text
        D---E  feature
       /
A---B---C  master
```

The feature branch was based on an older version of `master`.

### After Rebase

```text
A---B---C---D'---E'  feature
```

The feature commits were replayed on top of the latest `master`.

The commits receive new IDs because their parent commits have changed.

---

## Rebase vs Merge

### Merge

```text
A---B---C---------M
     \           /
      D---E------
```

Merge preserves the existing branch history and creates a merge commit.

### Rebase

```text
A---B---C---D'---E'
```

Rebase creates a linear history by replaying commits on top of another branch.

---

## Commands Practiced

| Command                           | Purpose                               |
| --------------------------------- | ------------------------------------- |
| `git init`                        | Initialize a Git repository           |
| `git checkout -b feature-rebase`  | Create and switch to a feature branch |
| `git checkout master`             | Switch to master                      |
| `git log --oneline`               | View commit history                   |
| `git log --oneline --graph --all` | Visualize all branch histories        |
| `git rebase master`               | Rebase the current branch onto master |
| `git status`                      | Check repository/rebase status        |
| `git add <file>`                  | Mark a conflict as resolved           |
| `git rebase --continue`           | Continue a rebase                     |
| `git rebase --abort`              | Cancel a rebase                       |
| `git rebase --skip`               | Skip a conflicting commit             |
| `git push --force-with-lease`     | Safely push rewritten history         |

---

## Key Learnings

### 1. Rebase rewrites history

Rebase does not simply move existing commits. It creates new commits based on a new parent.

### 2. Rebase keeps history linear

It can make project history easier to read because feature commits appear directly after the latest base branch commit.

### 3. Conflicts can happen during rebase

When Git cannot automatically apply a commit, the conflict must be resolved manually.

### 4. `git rebase --continue`

After resolving a conflict:

```bash
git add <file>
git rebase --continue
```

continues the rebase process.

### 5. `git rebase --abort`

If the rebase becomes problematic:

```bash
git rebase --abort
```

returns the branch to its state before the rebase started.

### 6. Use `--force-with-lease`

Because rebase changes commit history, a normal push may fail.

The safer option is:

```bash
git push --force-with-lease
```

rather than blindly using:

```bash
git push --force
```

---

## Conclusion

This task gave me practical experience with **Git Rebase**, including creating branches, making independent changes, rebasing a feature branch onto `master`, resolving a real merge conflict, continuing the rebase, and safely pushing the rewritten history to GitHub.



