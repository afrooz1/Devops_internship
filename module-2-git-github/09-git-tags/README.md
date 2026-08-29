# Task 9 — Using Git Tags

## Objective

The objective of this task was to understand and practice Git tags for marking specific points in Git history, especially for software releases and versioning.

In this task, I practiced:

* Creating a lightweight Git tag
* Listing Git tags
* Inspecting a tag
* Adding a remote repository
* Pushing a tag to a remote repository
* Verifying remote tags
* Deleting a local tag
* Deleting a remote tag
* Creating an annotated tag with a message
* Pushing and verifying an annotated tag

---

## 1. Initialize the Repository

Created the Task 9 directory and initialized it as a Git repository.

```bash
mkdir 09-git-tags
cd 09-git-tags
git init
```

Checked the repository status:

```bash
git status
```

The repository was initially on the `master` branch with no commits.

---

## 2. Create the Initial Commit

Created a `README.md` file:

```bash
echo "# Git Tags Practice" > README.md
```

Added and committed the file:

```bash
git add README.md
git commit -m "Initial commit"
```

Verified the working tree:

```bash
git status
```

Result:

```text
On branch master
nothing to commit, working tree clean
```

---

## 3. Create a Lightweight Tag

Created a lightweight tag named `v1.0.0`:

```bash
git tag v1.0.0
```

Listed the available tags:

```bash
git tag
```

Output:

```text
v1.0.0
```

A lightweight tag is a simple reference pointing to a specific commit.

---

## 4. Inspect the Tag

Used `git show` to inspect the tag:

```bash
git show v1.0.0
```

The tag pointed to the initial commit:


This demonstrated that the tag was pointing to the current commit.

---

## 5. Configure the Remote Repository

Initially, pushing the tag failed because no remote named `origin` had been configured.

Added the GitHub repository as the remote:

```bash
git remote add origin https://github.com/afrooz1/07-pull-push-remote.git
```

Verified the remote:

```bash
git remote -v
```

Output:

```text
origin  https://github.com/afrooz1/07-pull-push-remote.git (fetch)
origin  https://github.com/afrooz1/07-pull-push-remote.git (push)
```

---

## 6. Push the Lightweight Tag

Pushed the tag to the remote repository:

```bash
git push origin v1.0.0
```

The tag was successfully pushed:

```text
[new tag] v1.0.0 -> v1.0.0
```

---

## 7. Verify the Remote Tag

Verified the tag on the remote repository:

```bash
git ls-remote --tags origin
```

The remote contained:

```text
29a26485608e57cb01c8006603919e77807a86a1 refs/tags/v1.0.0
```

This confirmed that the lightweight tag had been successfully pushed.

---

## 8. Delete the Local Tag

To practice annotated tags using the same version number, deleted the local lightweight tag:

```bash
git tag -d v1.0.0
```

Output:

```text
Deleted tag 'v1.0.0'
```

---

## 9. Create an Annotated Tag

Created an annotated tag with a release message:

```bash
git tag -a v1.0.0 -m "Version 1.0.0 release"
```

Listed the tags:

```bash
git tag
```

Output:

```text
v1.0.0
```

---

## 10. Inspect the Annotated Tag

Used:

```bash
git show v1.0.0
```

The output showed:

```text
tag v1.0.0
Tagger: afrooz1

Version 1.0.0 release

commit 29a26485608e57cb01c8006603919e77807a86a1
```

This confirmed that the tag was now an **annotated tag**.

Unlike a lightweight tag, an annotated tag contains additional metadata such as:

* Tag name
* Tagger
* Date
* Tag message
* Referenced commit

---

## 11. Delete the Old Remote Tag

The remote repository still contained the original lightweight `v1.0.0` tag.

Deleted the remote tag:

```bash
git push origin --delete v1.0.0
```

Output:

```text
- [deleted] v1.0.0
```

This removed the old lightweight tag from the remote repository.

---

## 12. Push the Annotated Tag

Pushed the new annotated `v1.0.0` tag:

```bash
git push origin v1.0.0
```

Output:

```text
[new tag] v1.0.0 -> v1.0.0
```

The annotated tag was successfully pushed.

---

## 13. Verify the Annotated Tag on the Remote

Ran:

```bash
git ls-remote --tags origin
```

The final output was:

```text
7898a1830df78a419b83c555f649ff2a727a1caf refs/tags/v1.0.0
29a26485608e57cb01c8006603919e77807a86a1 refs/tags/v1.0.0^{}
```

The `refs/tags/v1.0.0` entry represents the annotated tag object.

The `refs/tags/v1.0.0^{}` entry represents the commit that the annotated tag ultimately points to.

This confirmed that the annotated tag was successfully stored on the remote repository.

---

## Lightweight vs Annotated Tags

### Lightweight Tag

```bash
git tag v1.0.0
```

A lightweight tag is a simple pointer to a commit.

```text
v1.0.0
   |
   v
Commit
```

### Annotated Tag

```bash
git tag -a v1.0.0 -m "Version 1.0.0 release"
```

An annotated tag is a Git object containing additional information.

```text
v1.0.0
   |
   v
Tag Object
   |
   +-- Tag name
   +-- Tagger
   +-- Date
   +-- Message
   |
   v
Commit
```

Annotated tags are generally preferred for official software releases because they provide release metadata.

---

## Commands Practiced

```bash
# Initialize repository
git init

# Check repository status
git status

# Create a commit
git add README.md
git commit -m "Initial commit"

# Create lightweight tag
git tag v1.0.0

# List tags
git tag

# Inspect tag
git show v1.0.0

# Add remote
git remote add origin https://github.com/afrooz1/07-pull-push-remote.git

# Check remote
git remote -v

# Push tag
git push origin v1.0.0

# View remote tags
git ls-remote --tags origin

# Delete local tag
git tag -d v1.0.0

# Create annotated tag
git tag -a v1.0.0 -m "Version 1.0.0 release"

# Delete remote tag
git push origin --delete v1.0.0

# Push annotated tag
git push origin v1.0.0
```

---

## Key Learnings

Through this task, I learned that Git tags are useful for identifying important points in a project's history, especially software releases.

For example:

```text
v1.0.0 → First stable release
v1.1.0 → New features
v1.2.0 → Improvements
v2.0.0 → Major release
```

I also learned the difference between lightweight and annotated tags.

A lightweight tag is a simple reference to a commit, while an annotated tag stores additional information such as the tagger, date, and release message.

I also learned that tags are not automatically pushed when using a normal `git push`. A specific tag can be pushed using:

```bash
git push origin v1.0.0
```

Finally, I learned how to verify remote tags using:

```bash
git ls-remote --tags origin
```

---

## Result

Task 9 — **Using Git Tags** was successfully completed.

I created, inspected, pushed, deleted, and verified both lightweight and annotated Git tags.

The final remote repository contains the annotated:

```text
v1.0.0
```

with the release message:

```text
Version 1.0.0 release
```
