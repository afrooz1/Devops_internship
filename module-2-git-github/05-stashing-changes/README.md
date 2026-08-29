# Task 5 — Stashing Changes

## Objective

Practice temporarily saving uncommitted changes using Git stash, making and committing different changes, restoring previously stashed work, handling a stash conflict, and removing the stash after it is no longer needed.

## Repository

```text
05-stashing-changes
```

## Concepts Practiced

* `git stash`
* `git stash list`
* `git stash apply`
* `git stash drop`
* `git status`
* `git diff`
* Resolving merge conflicts
* Staging resolved changes
* Committing resolved changes

## Practical Steps

### 1. Initialize the Repository

Created the Task 5 directory and initialized it as a Git repository.

```bash
mkdir 05-stashing-changes
cd 05-stashing-changes
git init
```

Checked the repository status:

```bash
git status
```

---

### 2. Create the Initial File

Created `app.txt` and added the initial application configuration.

```bash
touch app.txt
echo "Application Configuration" > app.txt
echo "Version: 1.0" >> app.txt
```

Verified the contents:

```bash
cat app.txt
```

The file contained:

```text
Application Configuration
Version: 1.0
```

---

### 3. Commit the Initial Version

Staged and committed the initial configuration.

```bash
git add app.txt
git commit -m "Add initial application configuration"
```

This created a clean starting point for the stash exercise.

---

### 4. Make Uncommitted Changes

Added a new environment configuration without committing it.

```bash
echo "Environment: Development" >> app.txt
```

Checked the changes:

```bash
git status
git diff
```

The new change was visible as an uncommitted modification.

---

### 5. Stash the Changes

Temporarily saved the uncommitted changes using:

```bash
git stash
```

Then checked the working tree:

```bash
git status
```

The working tree became clean because the changes were temporarily stored in the stash.

Verified the stash:

```bash
git stash list
```

The stash appeared as:

```text
stash@{0}
```

---

### 6. Make Different Changes

After stashing the previous work, made a different change to `app.txt`.

The application version was changed from `1.0` to `1.1`.

```bash
echo "Version: 1.1" > app.txt
```

Checked the changes:

```bash
git diff
```

Then committed the new changes:

```bash
git add app.txt
git commit -m "Update application version"
```

---

### 7. Apply the Stashed Changes

Checked that the original stash was still available:

```bash
git stash list
```

Applied the stash:

```bash
git stash apply
```

Git detected that the same file had changed in both the committed work and the stashed work.

This resulted in a merge conflict:

```text
CONFLICT (content): Merge conflict in app.txt
```

---

### 8. Resolve the Stash Conflict

Used:

```bash
git status
git diff
```

to inspect the conflict.

The conflict showed differences between the current version and the stashed version.

Resolved the file by keeping:

```text
Application Configuration
Version: 1.1
Environment: Development
```

The conflict markers were removed from `app.txt`.

Verified the resolved file:

```bash
cat app.txt
```

---

### 9. Stage and Commit the Resolution

After resolving the conflict, staged the file:

```bash
git add app.txt
```

Checked the status:

```bash
git status
```

Then committed the resolved changes:

```bash
git commit -m "Resolve stash conflict"
```

---

### 10. Remove the Stash

Because `git stash apply` does not automatically remove the stash, the stash was still present.

Checked it with:

```bash
git stash list
```

Then removed the stash:

```bash
git stash drop stash@{0}
```

Verified that the stash was removed:

```bash
git stash list
```

No stashes remained.

---

## Important Git Commands

| Command           | Purpose                                             |
| ----------------- | --------------------------------------------------- |
| `git stash`       | Temporarily saves uncommitted changes               |
| `git stash list`  | Displays all saved stashes                          |
| `git stash apply` | Restores a stash while keeping it in the stash list |
| `git stash pop`   | Restores a stash and removes it                     |
| `git stash drop`  | Removes a specific stash                            |
| `git stash clear` | Removes all stashes                                 |
| `git diff`        | Shows unstaged changes                              |
| `git status`      | Shows the current repository state                  |

## Key Learning

Git stash is useful when I have unfinished changes that I do not want to commit yet but need to temporarily set aside.

The workflow I practiced was:

```text
Make changes
     ↓
git stash
     ↓
Working tree becomes clean
     ↓
Make different changes
     ↓
Commit different changes
     ↓
git stash apply
     ↓
Resolve conflict if necessary
     ↓
git add
     ↓
git commit
     ↓
git stash drop
```

I also learned that `git stash apply` does **not** delete the stash automatically. The stash remains available until it is explicitly removed with `git stash drop` or `git stash clear`.

## Final Result

Task 5 successfully demonstrated how Git stash can be used to temporarily save unfinished work, switch to other work, restore the saved changes later, and handle conflicts that may occur when the same files have changed.

The working tree was left clean and the stash was successfully removed.
