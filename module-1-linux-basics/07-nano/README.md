# Task 7 — Using Nano

## Objective

The objective of this task was to become familiar with the Nano text editor and practice basic text editing operations from the Linux terminal.

The exercise focused on:

* Opening a file using Nano
* Editing text
* Saving a file
* Exiting Nano
* Searching inside a file
* Cutting text
* Pasting text
* Navigating inside a file
* Creating and editing a Bash script using Nano
* Adding comments to explain important Nano shortcuts

---


# 1. Nano Practice File

A practice text file was created and edited using Nano:

```text
practice.txt
```

It was opened using:

```bash
nano practice.txt
```

The file contained text explaining the purpose of the exercise.

Example:

```text
This file is created to practice nano text editor
to understand its functionality and features
```

The file was edited directly inside Nano and then saved.

---

# 2. Opening a File

Nano can be used to create a new file or open an existing file.

The general syntax is:

```bash
nano filename
```

For example:

```bash
nano practice.txt
```

If the file does not already exist, Nano opens a new empty file with that name.

If the file already exists, Nano opens it for editing.

---

# 3. Editing Text

After opening a file in Nano, text can be entered directly using the keyboard.

The cursor can be moved using the arrow keys and other navigation keys.

Text can be inserted, deleted, and modified directly inside the editor.

---

# 4. Saving a File

The Nano shortcut for saving a file is:

```text
Ctrl + O
```

Nano displays the filename and asks for confirmation.

Pressing `Enter` confirms the filename and saves the changes.

Nano displays this shortcut as:

```text
^O Write Out
```

---

# 5. Exiting Nano

The shortcut for exiting Nano is:

```text
Ctrl + X
```

Nano displays:

```text
^X Exit
```

If there are unsaved changes, Nano asks whether the changes should be saved before exiting.

---

# 6. Searching Inside a File

Nano provides a search function using:

```text
Ctrl + W
```

The shortcut is displayed as:

```text
^W Where Is
```

After pressing `Ctrl + W`, a search prompt appears at the bottom of the screen.

A search term can then be entered, and Nano moves to the matching text.

---

# 7. Cutting Text

Nano can cut the current line using:

```text
Ctrl + K
```

The shortcut is displayed as:

```text
^K Cut
```

The current line is removed from the document and stored so that it can be pasted later.

---

# 8. Pasting Text

Previously cut text can be pasted using:

```text
Ctrl + U
```

The shortcut is displayed as:

```text
^U Paste
```

This allows text that was cut with `Ctrl + K` to be inserted again.

---

# 9. Navigating Inside a File

Different keyboard controls can be used to navigate through a file.

| Key        | Function                        |
| ---------- | ------------------------------- |
| Arrow keys | Move the cursor                 |
| Home       | Move to the beginning of a line |
| End        | Move to the end of a line       |
| Page Up    | Move upward through the file    |
| Page Down  | Move downward through the file  |

Nano also provides additional keyboard shortcuts for navigation.

---

# 10. Bash Script Created Using Nano

A Bash script was created and edited using Nano:

```text
nano_script.sh
```

It was opened using:

```bash
nano nano_script.sh
```

The script contains comments explaining important Nano shortcuts and their purposes.

Examples of documented shortcuts include:

```bash
# Ctrl + O is used to save the file
# Ctrl + X is used to exit Nano
# Ctrl + K is used to cut the current line
# Ctrl + U is used to paste previously cut text
# Ctrl + W is used to search inside a file
# Home key moves the cursor to the beginning of a line
# End key moves the cursor to the end of a line
```

The purpose of creating the script through Nano was to demonstrate that Nano can be used to create and edit Bash script files, not only normal text files.

---

# 11. Understanding the `^` Symbol in Nano

One important concept learned during this task was the meaning of the `^` symbol displayed in Nano shortcuts.

For example, Nano displays:

```text
^G Help
^O Write Out
^W Where Is
^K Cut
^U Paste
^X Exit
```

The `^` symbol represents the **Ctrl key**.

Therefore:

```text
^G = Ctrl + G
^O = Ctrl + O
^W = Ctrl + W
^K = Ctrl + K
^U = Ctrl + U
^X = Ctrl + X
```

The `^` symbol does not mean that the `^` key itself should be pressed.

It is simply Nano's notation for the Control key.

---

# 12. Important Nano Shortcuts

| Shortcut   | Function                  |
| ---------- | ------------------------- |
| `Ctrl + G` | Display help              |
| `Ctrl + O` | Save / Write Out          |
| `Ctrl + W` | Search                    |
| `Ctrl + K` | Cut current line          |
| `Ctrl + U` | Paste previously cut text |
| `Ctrl + X` | Exit Nano                 |

---

# 13. Verification

The following Nano operations were practiced:

* ✓ Open a file
* ✓ Edit text
* ✓ Save a file
* ✓ Exit Nano
* ✓ Search inside a file
* ✓ Cut text
* ✓ Paste text
* ✓ Navigate inside a file
* ✓ Create a Bash script using Nano
* ✓ Add comments explaining Nano shortcuts

The files were created and tested from the Linux/WSL terminal.

---

# 14. Problems Encountered

No major technical problems were encountered during the task.

The main focus was becoming familiar with Nano's keyboard-based interface and understanding the meaning of the shortcut notation displayed at the bottom of the editor.

---

# 15. What I Learned

Through this task, I learned how to use Nano as a command-line text editor.

I learned how to:

* Open existing files with Nano.
* Create new files using Nano.
* Edit text directly from the terminal.
* Save files using `Ctrl + O`.
* Exit Nano using `Ctrl + X`.
* Search for text using `Ctrl + W`.
* Cut lines using `Ctrl + K`.
* Paste previously cut text using `Ctrl + U`.
* Navigate through a file using keyboard controls.
* Create and edit Bash scripts using Nano.
* Understand the `^` notation used by Nano for the Control key.

The main lesson from this task was that Nano provides a simple way to create and modify files directly from the Linux command line without requiring a graphical text editor.

---

# 16. Resources Used

* Nano text editor
* Linux terminal / WSL environment
* Nano built-in shortcut and help information
* ChatGPT as a tutoring resource for understanding Nano concepts and troubleshooting
* GeekForGeeks

---

# Conclusion

Task 7 provided practical experience with the Nano text editor and its keyboard-based commands. The task demonstrated how files and Bash scripts can be created, edited, searched, saved, and managed directly from the Linux terminal.

The completed exercise improved my understanding of command-line text editing and provided practical experience with commonly used Nano shortcuts.
