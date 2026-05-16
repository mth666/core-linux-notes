# Two ways to make Git hub repo - PCs
 A - Clone (GitHub first)
Best when: repo already has files in it, or you are on a new machine
How to: git clone URL, done, everything downloads ready to go

 B - Init (Local first)  
Best when: you already have files on your machine you want to push up
How to: git init, git remote add origin, git push -u origin main

# NAVIGATE AROUND
cd ~/Documents/syseng-notes/
# move into a folder, ~ means your home directory /home/username

cd ..
# go back one folder up

pwd
# shows your current location, stands for print working directory

ls
# lists files and folders in current location

ls -lah
# lists everything including hidden files with sizes, more detailed

# CREATE FOLDERS AND FILES
mkdir foldername
# creates a single folder

mkdir -p ~/Documents/syseng-notes/troubleshootingnotes
# creates folder and any missing parent folders along the way, -p means parents

touch filename.md
# creates a new empty file

echo "# title here" >> README.md
# writes a line of text into a file, creates the file if it doesn't exist

# ONE TIME SETUP — per machine
git config --global credential.helper store
# saves your token permanently so you never get asked for password again

git config --global user.name "username"
# tells git who you are, shows up in commit history

git config --global user.email "email@gmail.com"
# ties your commits to your github account

# ONE TIME SETUP - per new repo (if no README was checked on github)
git init
# turns current folder into a git repository

git remote add origin https://github.com/username/repoName.git
# connects your local folder to your github repo

git branch -M main
# renames default branch to main, github standard

git push -u origin main
# first push ever, sets upstream so future pushes just need git push

# EVERY TIME YOU UPDATE - the three commands forever
git add .
# stages all changed and new files, dot means everything in current folder

git commit -m "your message here"
# saves a snapshot locally with a description of what you did

git push
# sends the committed changes up to github

# BONUS COMMANDS - useful to know
git status
# shows what files are changed, staged, or untracked

git log --oneline
# shows your commit history in a clean single line format

git pull
# downloads latest changes from github, run this first when on another machine

git clone https://github.com/username/repoName.git
# downloads a repo from github to your current location

# FULL WORKFLOW - brand new repo from scratch
mkdir -p ~/Documents/syseng-notes/notes1 
*****-p stands for parents***** 
# WITHOUT -p, need two commands to create two directories (folders in windows)
mkdir ~/Documents/syseng-notes
mkdir ~/Documents/syseng-notes/troubleshootingnotes
# WITH -p, one command does both at once
mkdir -p ~/Documents/syseng-notes/troubleshootingnotes

# create the folder

cd ~/Documents/syseng-notes/REPONAME
# navigate into it

echo "# REPONAME" >> README.md
# create a readme file with a title

git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/username/repoName.git
git push -u origin main
# all setup done, repo is live on github

# then create .md files, write notes, save, then same commands
git add .
git commit -m "describe what added"
git push

### example work flows (make directories first or folders in windows world)

mkdir -p ~/Documents/ideas/funeral-business
mkdir -p ~/Documents/ideas/camping-hiking
mkdir -p ~/Documents/ideas/smart-home

then create files (either with touch or nano)
nano/touch ~/Documents/ideas/funeral-business/README.md
nano/touch ~/Documents/ideas/camping-hiking/README.md
nano/touch ~/Documents/ideas/smart-home/README.md

##### note
touch make empty file. nano make file and you can instantly add contents in it in terminal. :)

cd ~/Documents/ideas
git init
git add .
git commit -m "initial ideas dump — funeral, camping, smart home"

#### then connect to github repo
git remote add origin https://github.com/username/repoName.git
git branch -M main
git push -u origin main
##### Note, create github repo first before connecting obviously xD