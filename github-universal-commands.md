# GitHub + Git Quick Reference

## 0. The Golden Rule

Before committing:

1. Save your files (`Ctrl + S`)
2. Check what Git sees

```bash
git status
```

If Git doesn't see your changes, they cannot be committed.

---

# 1. Terminal Navigation

```bash
pwd
```

Shows current location.

```bash
ls
```

List files and folders.

```bash
ls -lah
```

Detailed list including hidden files.

```bash
cd foldername
```

Move into a folder.

```bash
cd ..
```

Go up one folder.

```bash
cd ~/Documents
```

Go to Documents folder.

---

# 2. Create Folders and Files

Create a folder:

```bash
mkdir foldername
```

Create nested folders:

```bash
mkdir -p ~/Documents/projects/my-notes
```

`-p` means "create parent folders if needed."

Create an empty file:

```bash
touch README.md
```

Create a file and add text:

```bash
echo "# My Project" > README.md
```

Edit a file from terminal:

```bash
nano README.md
```

Save in Nano:

```text
Ctrl + O
Enter
Ctrl + X
```

---

# 3. One-Time Git Setup (Per Computer)

Tell Git who you are:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Optional: store credentials locally

```bash
git config --global credential.helper store
```

---

# 4. Two Ways To Start A Repository

## Method A: GitHub First (Clone)

Use when:

* Repository already exists on GitHub
* Working on a new computer
* Joining an existing project

Download repository:

```bash
git clone https://github.com/username/repoName.git
```

Enter folder:

```bash
cd repoName
```

Done.

---

## Method B: Local First (Init)

Use when:

* Files already exist on your computer
* Creating a completely new project

Move into project folder:

```bash
cd ~/Documents/my-project
```

Initialize Git:

```bash
git init
```

Add files:

```bash
git add .
```

Create first commit:

```bash
git commit -m "first commit"
```

Rename branch:

```bash
git branch -M main
```

Connect GitHub repository:

```bash
git remote add origin https://github.com/username/repoName.git
```

Push:

```bash
git push -u origin main
```

Done.

---

# 5. Daily Workflow (The Three Commands Forever)

After editing files:

```bash
git add .
git commit -m "describe what changed"
git push
```

That's it.

---

# 6. Useful Commands

Check status:

```bash
git status
```

View commit history:

```bash
git log --oneline
```

Download latest changes:

```bash
git pull
```

See connected GitHub repository:

```bash
git remote -v
```

See files in current commit:

```bash
git ls-tree -r HEAD
```

---

# 7. Common Problems

## "fatal: not a git repository"

You forgot:

```bash
git init
```

or you're in the wrong folder.

Check:

```bash
pwd
ls -lah
```

Look for:

```text
.git
```

---

## "nothing to commit, working tree clean"

Git sees no changes.

Usually:

* File wasn't saved
* No files were modified

Check:

```bash
git status
```

---

## "Everything up-to-date"

Nothing new has been committed.

Did you:

1. Save file?
2. git add . ?
3. git commit -m "message" ?

---

## "main -> main (fetch first)"

GitHub already contains commits.

Usually:

```bash
git pull origin main --allow-unrelated-histories
```

Or overwrite remote:

```bash
git push --force
```

(Only if you're sure.)

---

# 8. Full New Project Example

Create project folder:

```bash
mkdir -p ~/Documents/ideas/funeral-business
cd ~/Documents/ideas/funeral-business
```

Create README:

```bash
echo "# Funeral Business Ideas" > README.md
```

Initialize Git:

```bash
git init
git add .
git commit -m "first commit"
```

Create GitHub repository online.

Connect:

```bash
git remote add origin https://github.com/username/business.git
git branch -M main
git push -u origin main
```

Project is now on GitHub.

Future updates:

```bash
git add .
git commit -m "added new ideas"
git push
```
---
# Markdown files Cheat Sheet

A quick reference for notes, documentation, GitHub repositories, and personal knowledge bases.

---

# Headers

```md
# Main Title

## Section

### Subsection

#### Smaller Section
```

Use:

* `#` = document title
* `##` = major section
* `###` = subsection
* `####` = rarely needed

Example:

```md
# Linux Notes

## Package Management

### apt

#### Common Commands
```

---

# Paragraphs

Just leave a blank line between paragraphs.

```md
This is paragraph one.

This is paragraph two.
```

---

# Horizontal Line

Creates a visual separator.

```md
---
```

Result:

---

Use often to break large notes into sections.

---

# Bold

```md
**Important**
```

Result:

**Important**

Good for warnings and key ideas.

---

# Italics

```md
*Optional*
```

Result:

*Optional*

---

# Bold + Italics

```md
***Very Important***
```

Result:

***Very Important***

---

# Inline Code

For commands, filenames, packages, etc.

```md
Use `git status` to check changes.
```

Result:

Use `git status` to check changes.

Examples:

```md
`git`
`README.md`
`docker`
`/etc/ssh/sshd_config`
```

---

# Code Blocks

For terminal commands or code.

````md
```bash
git status
git add .
git commit -m "update"
```
````

Result:

```bash
git status
git add .
git commit -m "update"
```

Common language tags:

````md
```bash
```

```python
```

```javascript
```

```yaml
```

```json
```
````

---

# Bullet Lists

```md
- Item One
- Item Two
- Item Three
```

Result:

* Item One
* Item Two
* Item Three

Nested:

```md
- Linux
  - Debian
  - Fedora
- Windows
```

Result:

* Linux

  * Debian
  * Fedora
* Windows

---

# Numbered Lists

```md
1. First
2. Second
3. Third
```

Result:

1. First
2. Second
3. Third

---

# Task Lists (Checkboxes)

Very useful for projects and study plans.

```md
- [ ] Learn Git
- [ ] Learn Docker
- [x] Install Debian
```

Result:

* [ ] Learn Git
* [ ] Learn Docker
* [x] Install Debian

---

# Quotes

```md
> Important note.
>
> Another line.
```

Result:

> Important note.
>
> Another line.

Good for:

* Book notes
* Ideas
* Warnings
* Definitions

---

# Links

```md
[GitHub](https://github.com)
```

Result:

[GitHub](https://github.com)

---

# Images

```md
![Alt Text](image.png)
```

Example:

```md
![Network Diagram](network-diagram.png)
```

---

# Tables

```md
| Tool | Purpose |
|------|---------|
| Git | Version Control |
| Docker | Containers |
| SSH | Remote Access |
```

Result:

| Tool   | Purpose         |
| ------ | --------------- |
| Git    | Version Control |
| Docker | Containers      |
| SSH    | Remote Access   |

---

# Collapsible Sections (GitHub Supports This)

```md
<details>
<summary>Click to Expand</summary>

Hidden content here.

</details>
```

Useful for:

* Long notes
* Troubleshooting sections
* Optional information

---

# Keyboard Shortcuts

## VS Code

```text
Ctrl + S      Save file
Ctrl + F      Find
Ctrl + H      Replace
Ctrl + /      Comment line
```

## Nano

```text
Ctrl + O      Save
Enter         Confirm filename
Ctrl + X      Exit
```

---

# Recommended Note Structure

````md
# Topic Name

Short description.

---

## Commands

```bash
command here
````

Explanation.

---

## Notes

* Important thing
* Another thing
* Warning

---

## Troubleshooting

### Problem

Description.

### Solution

```bash
fix command
```

````

---

# My Personal Rules

1. One topic = one file
2. Use lots of whitespace
3. Use `---` between sections
4. Use code blocks for commands
5. Use bullets instead of giant paragraphs
6. Keep sections short
7. Save files before Git commits (`Ctrl + S`)
8. Run `git status` before every commit

---

# Minimal Template

```md
# Topic Name

Short description.

---

## Commands

```bash
command here
````

---

## Notes

* Note 1
* Note 2

---

## Troubleshooting

### Problem

Description.

### Solution

```bash
fix command
```

```

This template works for:

- Linux notes
- Git notes
- Docker notes
- Programming notes
- Business ideas
- Personal knowledge bases
```
---