# Task 7 — Pulling and Pushing to a Remote Repository

## Objective

Practice working with remote Git repositories and understand how local and remote repositories synchronize.

In this task, I practiced:

* Connecting a local repository to a remote GitHub repository using `git remote add origin`.
* Pushing local commits to GitHub using `git push`.
* Understanding the `origin` remote.
* Understanding the difference between local branches and remote-tracking branches.
* Pulling changes from a remote repository using `git pull`.
* Using `git fetch` to retrieve remote changes.
* Understanding branch divergence.
* Creating and resolving a merge conflict.
* Using `git pull --no-rebase` to merge divergent branches.
* Resolving conflicts manually and creating a merge commit.
* Verifying that the local and remote repositories are synchronized.

---

## 1. Create Repository

Created a new directory for Task 7 and initialized it as a Git repository.

```bash
mkdir 07-pull-push-remote
cd 07-pull-push-remote
git init
```

Git initialized the repository using the `master` branch.

Verified the branch using:

```bash
git branch
```

The active branch was:

```text
* master
```

---

## 2. Create Initial README

Created `README.md` for the remote repository practice.

```text
# Git Remote Practice
```

Created the first commit:

```bash
git add README.md
git commit -m "Initial commit"
```

The repository now contained an initial commit.

---

## 3. Create a GitHub Remote Repository

Created a new GitHub repository:

```text
07-pull-push-remote
```

The remote repository was connected to the local repository using:

```bash
git remote add origin https://github.com/afrooz1/07-pull-push-remote.git
```

Verified the remote using:

```bash
git remote -v
```

The output showed:

```text
origin  https://github.com/afrooz1/07-pull-push-remote.git (fetch)
origin  https://github.com/afrooz1/07-pull-push-remote.git (push)
```

### Key Learning

`origin` is the conventional name given to the remote repository.

The command:

```bash
git remote add origin <repository-url>
```

connects the local repository to the remote repository.

---

## 4. Practice Authentication with GitHub

Initially attempted to push the repository using:

```bash
git push origin -u master
```

GitHub requested authentication:

```text
Username for 'https://github.com':
Password for 'https://afrooz1@github.com':
```

The push failed with:

```text
remote: Invalid username or token.
Password authentication is not supported for Git operations.
fatal: Authentication failed
```

### Key Learning

GitHub does not support using the normal GitHub account password for Git operations over HTTPS.

For HTTPS authentication, a GitHub Personal Access Token (PAT) can be used instead of the account password.

After authenticating successfully, the repository was able to communicate with GitHub.

---

## 5. Push Local Commits to GitHub

Used:

```bash
git push -u origin master
```

The `-u` option establishes an upstream relationship between the local `master` branch and the remote `origin/master` branch.

After setting the upstream branch, future pushes can be performed using:

```bash
git push
```

### Key Learning

The command:

```bash
git push origin master
```


The command:

```bash
git push -u origin master
```

also sets the upstream relationship.

---

## 6. Understand Local and Remote Branches

Used:

```bash
git branch -a
```

This showed the local branch and the remote-tracking branch.

Conceptually:

```text
Local branch:
master

Remote-tracking branch:
origin/master
```

### Key Learning

`master` is the local branch.

`origin/master` is Git's local reference to the remote `master` branch.

They are related, but they are not the same branch object.

---

## 7. Make a Local Change

Modified `README.md` locally.

The file contained:

```text
# Git Remote Practice
This line was changed locally.
This line was added remotely.
```

The local modification was committed:

```bash
git add README.md
git commit -m "Change README locally"
```

This created the local commit:

```text
c332495 Change README locally
```

---

## 8. Make a Remote Change

A separate change was made directly to `README.md` through the GitHub web interface.

The remote change created another commit:

```text
c15220b Update README with remote change note
```

At this point, the local and remote branches had different commits.


This is called **branch divergence**.

---

## 9. Practice `git fetch`

Used:

```bash
git fetch origin
```

This downloaded information about the latest remote commits without immediately merging those changes into the current local branch.

### Key Learning

`git fetch` retrieves changes from the remote repository and updates remote-tracking references such as:

```text
origin/master
```

It does not automatically modify the current working branch.

---

## 10. Initial `git pull` Attempt

Attempted:

```bash
git pull origin master
```

Git detected that the branches had diverged and displayed:

```text
hint: You have divergent branches and need to specify how to reconcile them.
fatal: Need to specify how to reconcile divergent branches.
```

Git suggested three possible strategies:

```bash
git config pull.rebase false
git config pull.rebase true
git config pull.ff only
```

### Key Learning

When local and remote branches have diverged, Git needs to know how the histories should be integrated.

For this task, I wanted to practice merging, so I used:

```bash
git pull --no-rebase origin master
```

---

## 11. Practice `git pull --no-rebase`

Used:

```bash
git pull --no-rebase origin master
```

Git successfully fetched the remote changes and attempted to merge them.

The output showed:

```text
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

Git detected a conflict because both the local and remote branches modified the same part of `README.md`.

---

## 12. Check the Merge Conflict

Used:

```bash
git status
```

Git reported:

```text
You have unmerged paths.
```

and:

```text
both modified: README.md
```

### Key Learning

A merge conflict occurs when Git cannot automatically determine which version of a change should be kept.

In this case:

```text
Local change
     +
Remote change
     ↓
Merge conflict
```

---

## 13. Resolve the Merge Conflict

Opened the conflicting file using:

```bash
nano README.md
```

The conflict markers inserted by Git were manually resolved.

The conflict markers look like:

```text
<<<<<<< HEAD
Local version
=======
Remote version
>>>>>>> origin/master
```

The unwanted conflict markers and content were removed, and the desired final version of the file was kept.

After resolving the file, checked the repository state using:

```bash
git status
```

---

## 14. Mark the Conflict as Resolved

Used:

```bash
git add README.md
```

This told Git that the conflict in `README.md` had been resolved.

### Key Learning

After manually resolving a conflict, `git add` tells Git:

> The conflict has been resolved and this is the version I want to include in the merge.

---

## 15. Create the Merge Commit

Created the merge commit using:

```bash
git commit -m "Resolve README merge conflict"
```

Git created the merge commit:

```text
bb7d360 Resolve README merge conflict
```

The history now contained both the local and remote changes.

The important commits were:

```text
bb7d360 Resolve README merge conflict
c15220b Update README with remote change note
c332495 Change README locally
3169885 Update README
8b7180f Update README.md
ec28159 Initial commit
```

### Key Learning

A merge commit combines two different lines of development into a single history.



---

## 16. Push the Merge Commit

After resolving the conflict and creating the merge commit, pushed the updated history to GitHub:

```bash
git push
```

Git successfully pushed:

```text
c15220b..bb7d360  master -> master
```

This updated the remote `master` branch with the merge commit.

---

## 17. Verify the Remote Repository

Ran:

```bash
git pull origin master
```

Git responded:

```text
Already up to date.
```

This confirmed that there were no new changes on the remote repository that needed to be downloaded.

---

## 18. Final Repository Status

Verified the repository using:

```bash
git status
```

The final result was:

```text
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean
```

This confirmed:

* The current branch is `master`.
* Local `master` is synchronized with `origin/master`.
* There are no uncommitted changes.
* The working directory is clean.

---

## 19. Final Branch

Used:

```bash
git branch
```

The result was:

```text
* master
```

This confirmed that the active branch was `master`.

---

## 20. Final Commit History

Used:

```bash
git log --oneline
```

Final history:

```text
bb7d360 (HEAD -> master, origin/master) Resolve README merge conflict
c15220b Update README with remote change note
c332495 Change README locally
3169885 Update README
8b7180f Update README.md
ec28159 Initial commit
```

The most important part is:

```text
bb7d360 (HEAD -> master, origin/master)
```

Both:

```text
master
```

and:

```text
origin/master
```

point to the same commit.

Therefore, the local and remote repositories are synchronized.

---

## 21. Important Git Commands

| Command                              | Purpose                                             |
| ------------------------------------ | --------------------------------------------------- |
| `git remote -v`                      | Displays configured remote repositories             |
| `git remote add origin <URL>`        | Connects a local repository to a remote repository  |
| `git fetch origin`                   | Downloads remote changes without merging            |
| `git pull origin master`             | Fetches and integrates changes from remote `master` |
| `git pull --no-rebase origin master` | Pulls and merges remote changes                     |
| `git push origin master`             | Pushes local `master` to remote `master`            |
| `git push -u origin master`          | Pushes and sets the upstream branch                 |
| `git push`                           | Pushes to the configured upstream branch            |
| `git branch -a`                      | Displays local and remote-tracking branches         |
| `git status`                         | Displays the repository state                       |
| `git log --oneline`                  | Displays compact commit history                     |
| `git log --oneline --graph --all`    | Displays commit history as a graph                  |

---

## 22. `git fetch` vs `git pull`

### `git fetch`

```bash
git fetch origin
```

Downloads remote information but does not automatically merge it into the current branch.


The current local `master` branch is not automatically changed.

---

### `git pull`

```bash
git pull origin master
```

Gets changes from the remote and integrates them into the current branch.


For this task, I used:

```bash
git pull --no-rebase origin master
```

to explicitly perform a merge.

---

## 23. Understanding `origin`

When adding the remote:

```bash
git remote add origin <repository-url>
```

`origin` is simply the name assigned to the remote repository.


The name `origin` is a common Git convention.

---

## 24. Understanding `master` vs `origin/master`

### `master`

The local branch:

```text
master
```

### `origin/master`

The remote-tracking reference for the remote branch:

```text
origin/master
```


---

## 25. Understanding Merge Conflicts

A merge conflict can happen when:

```text
Local branch
     |
     | modifies same content
     v
   File A

Remote branch
     |
     | modifies same content
     v
   File A
```

Git cannot automatically determine which change should be kept.

The developer must:

1. Open the conflicting file.
2. Examine both versions.
3. Decide what the final content should be.
4. Remove conflict markers.
5. Run `git add`.
6. Create the merge commit.
7. Push the resolved history.

The workflow is:

```text
git pull
   ↓
CONFLICT
   ↓
Edit file
   ↓
git add
   ↓
git commit
   ↓
git push
```

---

## 26. Common Conflict Markers

Git uses markers such as:

```text
<<<<<<< HEAD
Local changes
=======
Remote changes
>>>>>>> origin/master
```

They represent:

```text
<<<<<<< HEAD
        ↓
Current local version

=======
        ↓
Separator

>>>>>>> origin/master
        ↓
Incoming remote version
```

These markers must be removed when resolving the conflict.

---

## 27. Important Concepts Learned

### Remote Repository

A remote repository is a repository hosted somewhere such as GitHub or GitLab and used for collaboration and sharing code.

### Origin

`origin` is the conventional name for the remote repository.

### Push

```bash
git push
```

Sends local commits to the remote repository.

### Pull

```bash
git pull
```

Downloads remote changes and integrates them into the current branch.

### Fetch

```bash
git fetch
```

Downloads remote changes without automatically integrating them into the current branch.

### Branch Divergence

Branches diverge when both local and remote branches contain different commits that the other side does not have.

### Merge Conflict

A merge conflict occurs when Git cannot automatically combine changes, commonly because the same part of a file was modified differently.

### Merge Commit

A merge commit combines two different lines of development into one history.

---

## 28. Practical Workflow Learned

The complete workflow practiced in this task was:

```text
Create local repository
        ↓
git init
        ↓
Create and commit files
        ↓
Create GitHub repository
        ↓
git remote add origin URL
        ↓
git push -u origin master
        ↓
Make local changes
        ↓
Make remote changes
        ↓
git fetch origin
        ↓
git pull
        ↓
Branches diverge
        ↓
git pull --no-rebase origin master
        ↓
Merge conflict
        ↓
Resolve conflict manually
        ↓
git add README.md
        ↓
git commit
        ↓
git push
        ↓
git pull
        ↓
Already up to date
```

---

## What I Learned

Through this task, I learned how local Git repositories communicate with remote repositories such as GitHub.

I learned that:

* `git remote add origin` connects a local repository to a remote repository.
* `origin` is the conventional name for a remote repository.
* `git push` sends local commits to the remote repository.
* `git pull` retrieves and integrates remote changes.
* `git fetch` retrieves remote information without automatically merging it.
* `master` is the local branch while `origin/master` represents the remote-tracking branch.
* Local and remote branches can diverge when both contain different commits.
* `git pull --no-rebase` can be used to merge divergent histories.
* Merge conflicts occur when Git cannot automatically combine conflicting changes.
* Conflicts must be manually resolved before they can be committed.
* `git add` marks a resolved conflict as ready to be committed.
* A merge commit combines the local and remote histories.
* After resolving and pushing the merge, local and remote branches can become synchronized again.
* GitHub HTTPS authentication requires a supported authentication method such as a Personal Access Token rather than a normal account password.

This task gave me practical experience with remote repositories, pushing, pulling, branch divergence, merge conflicts, conflict resolution, and synchronization between local Git and GitHub.

---

## Task 7 Status

* `git remote add origin <repository-url>` ✓
* `git remote -v` ✓
* `git push -u origin master` ✓
* `git fetch origin` ✓
* `git pull origin master` ✓
* `git pull --no-rebase origin master` ✓
* Created a local/remote divergence ✓
* Created a merge conflict ✓
* Resolved a merge conflict ✓
* `git add README.md` ✓
* Created a merge commit ✓
* `git push` ✓
* Verified `git status` ✓
* Verified local and remote synchronization ✓
