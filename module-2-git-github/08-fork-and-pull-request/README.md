# Task 8 — Forking and Pull Requests

## Objective

The objective of this task was to practice the GitHub **fork and pull request workflow** used when contributing to repositories where we do not have direct write access.

In this task, I practiced:

* Forking a public repository
* Cloning my fork locally
* Understanding `origin`
* Creating a feature branch
* Making changes to a project
* Reviewing changes using `git status` and `git diff`
* Staging and committing changes
* Pushing a feature branch to my fork
* Creating a Pull Request to the original repository
* Understanding Pull Request structure and merge information
* Learning the purpose of `upstream`
* Understanding the Pull Request review process

---

## Repository Used

For this practical task, I used the public **Spoon-Knife** repository.

### Original Repository

```text
https://github.com/octocat/Spoon-Knife
```

### My Fork

```text
https://github.com/afrooz1/Spoon-Knife
```

---

# 1. Forking the Repository

I first forked the public `Spoon-Knife` repository to my GitHub account.

### Concept

A **fork** is a GitHub-hosted copy of another repository under my own GitHub account.

It allows me to work on a project without requiring direct write access to the original repository.

---

# 2. Creating the Task Directory

I created a directory for Task 8:

```bash
cd ~/module-2-git-github
mkdir 08-fork-and-pull-request
cd 08-fork-and-pull-request
```

---

# 3. Cloning My Fork

I cloned my fork instead of cloning the original repository:

```bash
git clone https://github.com/afrooz1/Spoon-Knife.git
```

Then entered the repository:

```bash
cd Spoon-Knife
```

I verified the files:

```bash
ls
```

Output included:

```text
README.md
index.html
styles.css
```

### Concept: Fork vs Clone

```text
Fork
GitHub Repository → GitHub Repository

Clone
GitHub Repository → Local Computer
```

A fork creates my GitHub copy, while `git clone` downloads that repository to my local machine.

---

# 4. Checking the Remote Repository

I checked the configured remote:

```bash
git remote -v
```

The remote was:

```text
origin  https://github.com/afrooz1/Spoon-Knife.git (fetch)
origin  https://github.com/afrooz1/Spoon-Knife.git (push)
```

### Concept: `origin`

`origin` is the default remote name Git assigns when a repository is cloned.

In this fork workflow:

```text
origin → my fork
```

Therefore:

```bash
git push origin task-8-contribution
```

means:

> Push the `task-8-contribution` branch to my GitHub fork.

---

# 5. Creating a Feature Branch

I created a separate branch for my contribution:

```bash
git checkout -b task-8-contribution
```

I verified the branches:

```bash
git branch
```

Output:

```text
  main
* task-8-contribution
```

The `*` indicates the currently active branch.

### Why Use a Feature Branch?

Feature branches keep changes isolated from the stable `main` branch.


This allows the changes to be reviewed before they are merged into `main`.

---

# 6. Inspecting the Existing File

I checked the contents of `index.html`:

```bash
cat index.html
```

The original file contained:

```html
<p>
  Fork me? Fork you, @octocat!
</p>
```

---

# 7. Making a Change

I edited `index.html` using Nano:

```bash
nano index.html
```

I added:

```html
<p>
  Changes Made by afrooz
</p>
```

After saving the file, I verified the change:

```bash
cat index.html
```

---

# 8. Checking Repository Status

I checked the current Git state:

```bash
git status
```

Git reported:

```text
On branch task-8-contribution
Changes not staged for commit:
    modified: index.html
```

This showed that Git detected my modification but it had not yet been staged.

---

# 9. Reviewing the Difference

Before committing, I used:

```bash
git diff
```

The diff showed:

```diff
 <p>
   Fork me? Fork you, @octocat!
 </p>
+<p>
+  Changes Made by afrooz
+</p>
```

### Concept: `git diff`

`git diff` allows us to inspect exactly what changed in tracked files before staging or committing them.

This is an important practice because it helps prevent accidental changes from being committed.

---

# 10. Staging the Change

I staged the modified file:

```bash
git add index.html
```

Then checked the status:

```bash
git status
```

Git showed:

```text
Changes to be committed:
    modified: index.html
```

The file was now in the **staging area**.

---

# 11. Committing the Change

I created a commit:

```bash
git commit -m "changes made"
```

Git created commit:

```text
4c105ae
```

The commit contained:

```text
1 file changed
3 insertions
0 deletions
```

After committing, I verified the working tree:

```bash
git status
```

Output:

```text
nothing to commit, working tree clean
```


---

# 12. Pushing the Branch

I pushed my feature branch to my fork:

```bash
git push -u origin task-8-contribution
```

Git created the remote branch:

```text
task-8-contribution -> task-8-contribution
```

The branch now existed on my GitHub fork.


### Concept: `-u`

The `-u` option establishes an upstream tracking relationship between the local branch and its remote branch.

After setting it, future pushes can usually be performed with:

```bash
git push
```

instead of specifying the remote and branch every time.

---

# 13. Creating the Pull Request

After pushing the branch, GitHub provided the option to create a Pull Request.

I created a Pull Request from:

```text
afrooz1:Spoon-Knife
task-8-contribution
```

into:

```text
octocat:Spoon-Knife
main
```

### Pull Request

The Pull Request was:

```text
#41014
```

Title:

```text
Update index.html with contributor information
```

The PR contained:

* 1 commit
* 1 changed file
* 3 additions
* 0 deletions

---

# 14. Pull Request Description

I documented the changes and verification performed:

```text
## Changes Made

- Updated `index.html`
- Added contributor information
- Practiced the fork, branch, commit, push, and pull request workflow

## Testing

- Verified the changes using `git diff`
- Confirmed the working tree was clean after committing
- GitHub reports that the branches can be automatically merged
```

---

# 15. Understanding the Pull Request

GitHub displayed:

```text
afrooz1 wants to merge 1 commit into octocat:main
from afrooz1:task-8-contribution
```


A Pull Request is a request for the maintainers of the original repository to review and potentially merge my changes.

---

# 16. Push vs Pull Request

These two concepts are different.

### `git push`

Uploads commits from my local repository to a remote repository:

Example:

```bash
git push origin task-8-contribution
```

### Pull Request

Requests that changes from one branch be reviewed and potentially merged into another branch:


Therefore:

> `git push` sends the commits.

> A Pull Request requests review and merging of those commits.

---


# 17. Pull Request Review Concepts

A Pull Request can be reviewed by other developers before merging.

A reviewer can inspect:

* Conversation
* Commits
* Files changed
* Code differences
* Automated checks
* Merge conflicts

GitHub provides review actions such as:

### Comment

Provide feedback without approving or rejecting the PR.

### Approve

Indicate that the changes look good.

### Request changes

Indicate that changes should be made before the PR is merged.

---

# 18. Code Review

When reviewing another developer's Pull Request, important things to check include:

### Correctness

Does the change solve the intended problem?

### Bugs

Could the changes introduce unexpected behavior?

### Readability

Is the code easy for other developers to understand?

### Security

Are passwords, tokens, API keys, or other sensitive values exposed?

### Maintainability

Will the code remain understandable and maintainable?

### Testing

Are appropriate tests included?

### Documentation

Does the change require documentation updates?

---

# 19. Example Review Comment

A constructive code review comment should explain both the problem and a possible improvement.

For example, if a developer writes:

```javascript
const password = "passwod123";
```

A reviewer could comment:

```text
This appears to expose a password directly in the source code.
Could we move this value to an environment variable instead?
```

A good review should be:

* Specific
* Constructive
* Respectful
* Focused on the code

---

# 20. Pull Request Merge Status

GitHub reported:

```text
No conflicts with base branch
Changes can be cleanly merged.
```

This means GitHub determined that the changes could be merged automatically without resolving a merge conflict.



If both branches modified the same lines in incompatible ways, GitHub could instead report a merge conflict that would need to be resolved.

---


# 21. Commands Practiced

```bash
cd ~/module-2-git-github

mkdir 08-fork-and-pull-request

cd 08-fork-and-pull-request

git clone https://github.com/afrooz1/Spoon-Knife.git

cd Spoon-Knife

git remote -v

git checkout -b task-8-contribution

git branch

cat index.html

nano index.html

git status

git diff

git add index.html

git status

git commit -m "changes made"

git status

git push -u origin task-8-contribution
```

---

# 22. Key Concepts Learned

| Concept        | Meaning                                                  |
| -------------- | -------------------------------------------------------- |
| Fork           | GitHub copy of another repository under your account     |
| Clone          | Downloads a remote repository to the local machine       |
| Origin         | Usually the remote repository representing your fork     |
| Upstream       | Usually the original repository                          |
| Branch         | Isolated line of development                             |
| Commit         | Snapshot of changes in Git history                       |
| Push           | Uploads local commits to a remote repository             |
| Pull Request   | Request to review and merge changes                      |
| Diff           | Shows what changed between versions                      |
| Code Review    | Examination of proposed changes before merging           |
| Merge Conflict | Situation where Git cannot automatically combine changes |

---

# 23. Interview Questions

### What is a fork?

A fork is a copy of a repository under my GitHub account that allows me to work on the project without requiring direct write access to the original repository.

### What is the difference between fork and clone?

A fork creates a repository copy on GitHub, while cloning downloads a repository from a remote server to the local machine.

### What is `origin`?

`origin` is the default remote name assigned when cloning a repository. In a fork workflow, it usually points to my fork.

### What is `upstream`?

`upstream` is commonly used to refer to the original repository from which a fork was created.

### What is a Pull Request?

A Pull Request is a request to have changes from one branch or repository reviewed and potentially merged into another branch.

### Does `git push` create a Pull Request?

No. `git push` uploads commits to a remote repository. A Pull Request is then created to request review and merging of those changes.

### Why use feature branches?

Feature branches isolate development work from the stable branch and allow changes to be reviewed before merging.

### What does `git diff` do?

`git diff` shows changes between versions of files so they can be inspected before committing.

### What can a reviewer do on a Pull Request?

A reviewer can comment, approve the Pull Request, or request changes.

---

# 24. Task Status

* [x] Fork a public repository
* [x] Clone the fork locally
* [x] Create a feature branch
* [x] Make changes
* [x] Check changes with `git status`
* [x] Review changes with `git diff`
* [x] Stage changes
* [x] Commit changes
* [x] Push branch to fork
* [x] Create Pull Request
* [x] Understand PR commits and file changes
* [x] Understand merge status
* [ ] Review another developer's Pull Request
* [ ] Leave a review comment on another developer's Pull Request

