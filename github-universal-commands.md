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
git remote add origin https://github.com/username/funeral-business.git
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
