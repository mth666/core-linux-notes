Linux Notes to By Heart :)

# making users and adding to sudo list (sudoer list). 
-------------------------------------------------------------------
in Debian 
usermod -aG sudo username

in Red hat world
usermod -aG wheel username

*** in Red Hat world, sudo group is called wheel. 
-------------------------------------------------------------------

# Networking commands 
---------------------
to see, brief ip address, use ip -brief address or ip -br a
to see your public IP (the one the internet sees), you can't use ip addr. You can use this "human-friendly" shortcut:
curl ifconfig.me


checking gate way
ip route
Look for the line starting with default via. This tells you where your traffic goes to reach the internet.


## Check if a specific port is listening
ss -tunlp
Meaning of the flags: It helps to remember them as "Tunnel-P."	
t: TCP connections
u: UDP connections
n: Numeric (shows port numbers like 80 instead of "http")
l: Listening ports only
p: Process (shows which program is using the port

This replaces the old netstat command. It shows you which services are currently running and waiting for connections on your server.
-------------------------------------------------------------------
# Users
-----
Permissions: useradd vs adduser
"friendlier" version of the command for creating users. 

useradd: The raw, low-level command. 
It doesn't create a home folder or ask for a password by default.

adduser: The "Debian-way." It is a script that asks you questions (Name? Password? Room number?) and sets everything up for you automatically.
user adding 
examples
useradd myo
passwd myo

manually create homefolders for created users 
sudo useradd -m username
example : sudo useradd -m myo
The -m flag tells RHEL to create the home directory that useradd command usually skips

Setting password for the created users 
sudo passwd username
example : sudo passwd myo

deleting user 
sudo userdel -r username
example : sudo userdel -r myo
-------------------------------------------------------------------
sharp sign  # vs dollar sign $
the # that means root. $ means regular user. 
-------------------------------------------------------------------

# essential core commands to master and fluent
----------
tail		
example : 
```bash 
tail -f /var/log/syslog 
```
use case : shows the last few lines of a file. however, its most important feature is the -f flag (which stands for "follow").
Real-world use:  when trying to figure out why a website is not loading. open the "error logs" with tail -f. 

every time a new error happens, it pops up on the screen in real time.
-------------------------------------------------------------------
## grep 		Search for text
By default, grep is very strict. If you search for "apple," it will not find "Apple."

the "ignore case" Flag (-i)
example Command: 
```bash
grep -i "apple" fruitlist.txt
```
output : It finds apple, Apple, APPLE, and aPpLe.


the "line number" Flag (-n)
When looking at a long configuration file with 500 lines, and need to know where the word is. -n can use it. 
Command: grep -n "error" system.log
results: It shows the line of text and the line number (e.g., Line 42: error found).

-r The "Recursive" Flag (-r)
this is for searching everything when don't know which file contains the word. It searches every file in a folder and every sub-folders.
command: 
```bash
grep -r "Gemini" /home/morty/projects/
```
-v The "Invert" Flag (-v)
This is the "Everything But" flag. It shows every line that does not contain the word.
command: 
```bash
grep -v "success" results.txt
```

```bash
-c The "Count" Flag (-c)
```
Sometimes you don't want to see the text; you just want to know how many times a word appears.
Command: 
```bash
grep -c "failed" login_attempts.log
```
result: It gives a simple number, likes 15.

-w Whole Words (-w)
If you search for grep "art", it will find "art," but also "party," "smart," and "chart."
If only want the specific word "art," use the -w flag:
```bash
grep -w "art" filename.txt
```
practice examples 
```bash
grep "username" /etc/passwd
```
(This searches the system file that keeps track of users).

find how many "nologin" users exist:
```bash
grep -c "nologin" /etc/passwd
```
Search for a word regardless of capital letters in the software list:
```bash
grep -i "network" /etc/services
```
```bash
sudo grep -i "failed" /var/log/messages | wc -l
```
wc means word count 
-l means count lines
command in plain english : search /var/log/messages for any line containing the word failed in any capitalisation, then count how many lines matched.
### common grep flags summarized
```bash
grep "text" file          basic search
grep -i "text" file       case insensitive
grep -v "text" file       invert, exclude matches
grep -c "text" file       count matching lines
grep -n "text" file       show line numbers
grep -r "text" /path      search recursively
```
-------------------------------------------------------------------
# find (The Partner to Grep)
--------
grep command searches for text inside a file, find searches for the file itself.
real world use case: when you know there is a configuration file somewhere in the /etc folder, but couldn't remember the exact name or where it is.
to find everything ending in .ssh: 

```
sudo find / -name "*.ssh"
```

mechanics:
/: tells the command to start at the very beginning of the filesystem (the "trunk" of the tree or / )
-name: tells that we are searching for a specific pattern

*: this is a wildcard that matches any string of characters, just like in windows

sudo: essential when searching the whole drive, as it gives the permissions to look into protected system folders

```bash
example 1 : find /etc -name "*.conf"
example 2 : find /home/$USER/.ssh -name "id_*"
example 3 : sudo find / -name "*.ssh"
```
how its works: this searches specifically in the users hidden .ssh folder for any file starting with "id_"	

note: this helps locate the file across entire hard drive.

### example commands
```
find . -name "file.txt"     #find by name 
find /home -iname "Document.pdf"    #case insensitive search 
find /var -type d -name "config"    #find only directories
find / -size +100M (over 100MB)     #find large files
find ~ -mtime -7    #find recently modified (last 7 days in this)
find . -perm 644    #find by permission
find . -empty   #find empty files/items
```
find command can also perform actions on every file it identifies
find /tmp -name "*.tmp" -delet  #delete files
find . -name "*.txt" -exec chmod 644 {} \;  #execute a command (the {} is a placeholder for each files found)
find . -type f -exec grep -l "search_term" {} \;    #find file content: combined with grep to find text inside files
### using the locate command (quick way)
locate is much faster than find because it uses a pre-built database

command: locate id_ed25519 (or locate id_rsa)
Tips: If you just created the keys, locate might not see them yet because the database usually updates only once a day
To solve this, need to 
```
run sudo updatedb 
```
to refresh the system memory

-------------------------------------------------------------------
# tail (the live monitor)
--------

tail shows you the last few lines of a file. However, its most important feature is the -f flag (which stands for "follow").
Real-world use case: You are trying to figure out why a website is not loading. You open the "error log" with tail -f. Every time a new error happens, it pops up on your screen in real time.
Simple example: tail -f /var/log/syslog
Why it is essential: It is how engineers watch a system "breathe" and catch errors the moment they happen.
### example tail commands
```
tail -f 	Watch a file grow
tail by itself doesnt show all lines. It shows the last 10 lines by default. Thats it. 10 is the default number baked into the command.
tail > last 10 lines, static
tail -n 50 > last 50 lines, static
tail -f >  last 10 lines, then follows live
```
-------------------------------------------------------------------
# cat and less (The Viewers)
--------
cat prints the whole file to your screen. But when a file is 10,000 lines long, cat is messy. That is when you use less.
Real-world use: Reading a manual or a long script without cluttering your screen.
Simple example: 
```bash
less huge_file.txt
```
Why it is essential: less allows you to scroll up and down and search for words (using the / key) inside the viewer. It is much cleaner than cat.

less		Read a file slowly

-------------------------------------------------------------------
# df and du (The Space Managers)
--------
In the real world, servers often "break" because the hard drive is 100% full. These two commands  where the space went.
df -h (Disk Free): Shows how much total space is left on the disks.
du -sh (Disk Usage): Shows you how much space a specific folder is taking up.
note:  use these constantly to prevent the system from crashing due to a full hdd/ssd drive.

df (Disk Free use case): Overview, It tells if the whole disk is full.
du (Disk Usage use case): Specifics, It tells which directory/files inside the disk is taking up the most space.
```bash
df -h		
```
-h for human friendly readable
```
du -sh /var/*
```
-s for summary
-h for human friendly readable

-------------------------------------------------------------------
# Process Management
--------
to find the program that is eating 100% of your CPU use : top, htop, or ps commands

## htop (The Interactive Monitor)
Most modern distros don't come with htop by default, but it’s the first thing every engineer installs. It’s a color-coded, live-updating view of your CPU, RAM, and every running process.
Pro-Tip: If a process is frozen, you don't need to type a command to kill it. In htop, just use your arrow keys to highlight the "jerk" process and press F9 to kill it.

## ps aux (the "snapshot")
Sometimes you don't want a live view; you just want a list of everything running right now so you can grep it.
The Breakdown:
a = show processes for all users.
u = display the user/owner of the process.
x = show processes not attached to a terminal (background tasks).
Real World use case: combined this with grep to find a specific app.
```bash
ps aux | grep nginx
```
this tell: ss the web server actually running? who started it? when did it start?

## kill -9 (the "Im Not Asking" command)
When a program refuses to clos and need to terminate.
the workflow: find the PID (Process ID number) using ps or htop, then run:
```
sudo kill -9 1234 (Replace 1234 with the actual PID).
```
note: never use -9 first. try just 
```
kill [PID] 
```
(which is a polite) before using -9 (which is "Die Immediately")

Tips 
*** read the logs first: Before change a single setting or restart a service, use 
```
tail -f or grep skills on the logs in /var/log/. 
```
The server will almost always tell exactly why itss unhappy if you listen to it.

*** Config Files: Copy Before You Edit: Before you touch a config file (like /etc/ssh/sshd_config), make a backup.
```bash
sudo cp config_file config_file.bak
```
*** If you mess up the syntax and the server won't start, you just copy the .bak back and you're saved.

*** The "Tab" Key is Your Best Friend: Never type out a full file path like /etc/systemd/system/. Type /et[TAB]/sys[TAB]. If the terminal doesn't auto-complete, it means you made a typo or the file doesn't exist. It's a built-in "Check Engine" light.

-------------------------------------------------------------------

# Understanding the Process Lifecycle
--------
Every process has a state. When you run ps aux or top, look at the STAT column.

R (Running/Runnable): It is using the CPU right now or waiting for its turn.

S (Interruptible Sleep): It is waiting for something to happen (like you pressing a key). This is normal.

D (Uninterruptible Sleep): This is the "danger" state. The process is waiting for hardware (like a slow hard drive). You cannot kill a process in state D.

Z (Zombie): The process is technically dead, but its parent hasn't acknowledged it yet. It stays in the process table like a ghost.

-------------------------------------------------------------------
# Priority and Niceness 
--------
In Linux, we use "Niceness" to tell the CPU which processes are more important.Range: -20 (Highest priority) to 19 (Lowest priority/Very "Nice").
The Formula: The kernel calculates the actual priority ($PR$) using the niceness ($NI$) value:$$PR = 20 + NI$$Real 
World Tip: If you have a backup script running that is slowing down the web server, you "renice" it to 19 so it only uses the CPU when the server is idle.

-------------------------------------------------------------------

# Services, Permissions, and Package Management
--------
## Service Management (systemctl)
In the modern Linux world (Debian, RHEL, Rocky), we use systemd to manage services. This is how you start a web server, a database, or a custom script so it runs in the background.

Status: systemctl status nginx (Is it running? Did it crash?)

Start/Stop: systemctl start nginx or systemctl stop nginx

### The "Survivor" Flags:
Enable: systemctl enable nginx (This makes the service start automatically when the computer boots up).
Disable: systemctl disable nginx (It won't start on boot).

### The "Fixer" Flag:
Restart: systemctl restart nginx (Stops and starts it—fixes 90% of weird glitches).

-------------------------------------------------------------------

# File Ownership and Permissions (chown and chmod)
--------
 the # (root) vs $ (user) symbols. Understanding who owns what is the "security" part of the job.

## chown (Change Owner)
This changes who "owns" the file.

Command: sudo chown morty:morty file.txt

Translation: "Make the user 'morty' and the group 'morty' the owners of this file."

## chmod (Change Mode/Permissions)
changes what the owner, the group, and everyone else can do to the file.

The Numbers:

4 = Read (r)
2 = Write (w)
1 = Execute (x)

common 
755 — scripts and programs
644 — config files and documents
700 — private scripts, owner only
600 — private files, owner only, like SSH keys
Common Setup: chmod 755 script.sh

7 (4+2+1) = Owner can do everything.
5 (4+0+1) = Group and Others can only read and run it.

chmod XYZ - Always in this order, first owner, second group and third everyone else
X = you (the owner)
Y = the group assigned to the file
Z = everyone else on the system

# how many groups are there?
Every file has exactly one group assigned to it. Not multiple. Just one.
To see who owns a file and which group it belongs to:
```bash
ls -l /etc/nginx/nginx.conf
```
Output looks like:
-rw-r--r-- 1 root root 1234 May 12 09:00 nginx.conf
That's:

Owner = root
Group = root
Permissions = 644

So Y in  chmod is always just that one assigned group. Not all groups on the system, 
just the one attached to that specific file.

## why chmod if chown already did the job?
Because they do completely different things.
chown answers who owns it.
chmod answers what can they do with it.
Ownership and permissions are separate things. Think of it like a house:

chown = putting your name on the deed. You own it.
chmod = deciding which doors are locked. What can people do inside.

You can own a file but still not be able to write to it if permissions are wrong. They work together, neither one replaces the other.

do I need to be root first?
You don't need to literally log in as root. That's actually considered bad practice on modern Linux. Instead you stay as your normal user and use sudo in front of commands that need root power.
```bash
sudo chown root:root /etc/nginx/nginx.conf
sudo chmod 644 /etc/nginx/nginx.conf
```
sudo = "do this one command as root." Then you're back to being a normal user again.
Think of it like a key card at work. You're still you, but the key card gives you access to the server room for that one moment. You don't become the building manager permanently.
That's why your notes say the # prompt means root and $ means regular user. If you're logged in as full root all the time, one typo can destroy everything. sudo keeps you safe.

-------------------------------------------------------------------
# two ways of chmod (change mode) permissions. Absolute and Relative
--------
Choosing between numbers (Absolute mode) and letters (Relative mode) depends entirely on whether you want to wipe the slate clean or just tweak existing permissions.

## When to use the Number System (Absolute Mode)
--------
Use numbers when you want to set exact, complete permissions and do not care what the file's current permissions are. This completely overwrites the file's security settings.Standard Use Cases:Use 755 for Programs and Folders:Result: Owner can read/write/execute; everyone else can only read/execute.When: Use on executable tools (like your yt-dlp binary), custom bash scripts you want to run, or public directories.Use 644 for Regular Files:Result: Owner can read/write; everyone else can only read (nobody can execute).When: Use on text files, configuration files, images, or source code files.Use 600 for Private Files:Result: Only the owner can read/write; absolutely nobody else can see or touch it.When: Use on sensitive data like SSH keys, passwords, or private configuration files.

## When to use the Letter System (Relative Mode)
--------
Use letters when you want to modify just one specific permission without altering or messing up the other existing permissions on the file.Standard Use Cases:Use +x (or a+x) to quickly make something runnable:When: You just downloaded a script or tool and you want a fast way to make it executable without thinking about numbers.Use go-w to lock down a file:When: You have a shared file, and you want to instantly remove write permissions for the Group and Others, but leave everything else exactly as it was.Use u+w to make a file writable for yourself:When: You are getting a "Permission Denied" error trying to edit your own file, and you want to grant yourself write access without altering permissions for other users.

## chmod — Numbers vs Letters: When to Use Which?

| Scenario | Best Choice | Why |
|---|---|---|
| Creating a brand new script or deploying a system binary | Numbers `755` or `644` | Establishes a known, secure baseline from scratch |
| Making a quick tweak to a single file | Letters `+x` or `-w` | Fast, easy to remember, won't accidentally overwrite other safe permissions |
| Securing highly private data like SSH keys | Numbers `600` | Guarantees group and public access are entirely locked out |
												
-------------------------------------------------------------------
# Package Management (The "App Store" of Linux)
--------
to be fluent, need to learn both in dnf (the tool for RHEL/Rocky) alongside apt (for Debian).
# apt vs dnf  Common Tasks

| Task                | Debian (`apt`)                  | RHEL/Rocky (`dnf`)          |
|---------------------|---------------------------------|-----------------------------|
| Update lists        | `sudo apt update`               | `sudo dnf check-update`     |
| Install app         | `sudo apt install htop`         | `sudo dnf install htop`     |
| Remove app          | `sudo apt remove htop`          | `sudo dnf remove htop`      |
| Search for app      | `apt search nginx`              | `dnf search nginx`          |
-------------------------------------------------------------------
truncate -s 0
turncate = resize 
-s means size
0 means Zero bytes. 
-------------------------------------------------------------------
# Pipe | vs && and &
--------
Pipe |  takes the output of the first command and feeds it as input to the second command. They're connected, talking to each other.
```bash
ps aux | grep nginx
```
this comamnds means "Give me all processes, then filter that list for nginx"

&&  runs the second command only if the first one succeeded. They're independent, just chained together conditionally.
```bash
systemctl stop apache2 && systemctl start nginx
```
this command means "Stop apache and ONLY if that worked, start nginx"

easier to remember with this analogy
| = a conversation between commands. Output of one becomes input of next.
&& = a checklist. Do this, and if it passed, do the next thing.
does the first command produce output that the second command needs to read as input?
If yes → |
If no → && or just run them separately

&
When you run a command normally in the terminal, it blocks you. You can't type anything else until it finishes. Your terminal is frozen waiting for it to complete.
Like this: ping google.com 
your terminal just sits there pinging forever. You can't do anything else until you CTRL-C out of it.
But sometimes you have a task that takes a long time — a backup, a big file download, a database dump. You don't want to sit there staring at it. You want to kick it off and keep working. So you add & at the end:
rsync -r /home/user /backup/location &
That & throws it into the background. Your terminal prompt comes back immediately and you can keep doing other things while rsync runs behind the scenes.
Then to check on your background jobs:
jobs 
Shows you everything running in the background.
Think of it like this:

Normal command = you're on the phone with someone, can't do anything until they hang up
& = you put them on hold, go do other stuff, they're still there in the background

| = pass data between commands, take output from first commands then give to next commands. example : cat /var/log/nginx/error.log | grep "failed"
&& = do this, then if it worked do that
& = do this in the background, I'm not waiting around
-------------------------------------------------------------------
# making SSH keys
--------
The good news is that the commands for managing SSH keys are identical on both Debian and Red Hat (RHEL) systems, as they both use the OpenSSH suite.

## Generating the Key (The "Stamp")
To create a new pair of keys (a public one and a private one), use the ssh-keygen command. highly recommended to use the modern ED25519 algorithm for better security and performance:
```bash
ssh-keygen -t ed25519
```
What happens: The system will ask where to save the key (default is usually /home/user/.ssh/id_ed25519).
Passphrase: It will ask for a passphrase. This is an optional "password for your key" for extra safety. If you want a "silent" login, just press Enter twice to leave it empty.

## Copying the Key to the Server (The "Lock")
Since you are on a Debian host wanting to access a RHEL VM, you can "ship" your public key to the remote machine using:
```bash
ssh-copy-id batman@<RHEL_VM_IP>
```
How it works: This command logs into the RHEL VM once using your password, places your public key in the ~/.ssh/authorized_keys file, and then logs out. From then on, you won't need a password to log in.

Tips:
Safety First: the importance of backing up configuration files is essential
Before  start making major changes to how SSH works on the server side, remember to run: 
```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
```
Package Management: If the SSH tools are missing on a fresh install, remember the distribution's high-level tools: use apt for Debian and dnf for RHEL

-------------------------------------------------------------------
# making many SSH keys
--------
To create five distinct keys for  different VMs, use the -f flag to give each one a unique name. For example:
```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_debian
ssh-keygen -t ed25519 -f ~/.ssh/id_rhel
```
...and so on.

## Key Considerations:
One Key vs. Many Keys: In most professional environments, it is standard practice to use one private key on host machine and "ship" the corresponding public key to all five VMs using ssh-copy-id. This allows to manage one "identity" across multiple servers. only need five different keys if want to strictly isolate access so that losing one key doesn't compromise all five systems.
The "Check Engine" Light: When managing these multiple files in .ssh directory, remember the to use the Tab key to auto-complete the filenames
If it doesn't auto-complete, likely made a typo or the file wasn't created

Backups are Crucial: 
Before  start generating a bunch of new keys and potentially overwriting existing ones, it's a good idea to back up current .ssh folder: 
```bash
cp -r ~/.ssh ~/.ssh_backup
```

Managing the Connection: If you use different keys, you will have to tell SSH which key to use for which VM. You can do this with the -i flag: 
```bash
ssh -i ~/.ssh/id_rhel batman@<RHEL_IP>
```
In the Linux world, your private key is essentially your digital "identity" or passport. Just as you don't need five different passports to visit five different countries, you don't necessarily need five different keys for five different OSs. Instead, you keep your single private key secure on your host machine (your Debian 13) and simply place the corresponding public key on every VM you want to access.

shipping ssh keys to remote servers
example : 
```bash
ssh-copy-id username@<RHEL_VM_IP>
```
example 2 : 
```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub username@<RHEL_VM_IP>
```
-i flag: This specifies the exact path to the public key to copy.
flow : It will log into the RHEL VM once using password, automatically create the ~/.ssh directory if it doesn't exist, and append the public key to the authorized_keys file.

-------------------------------------------------------------------

# When to make Multiple SSH Keys (The "Isolation" Model)
--------
Security Silos: If you are managing a VM for a client and a separate VM for personal use, you might use different keys so that if one environment is compromised, the other remains safe.
Shared Environments: If multiple people need access to your RHEL VM, they should each have their own unique key, rather than sharing yours.

Tips:
Protect the "Master Key": Since one key can open all your doors, the security of your host machine is paramount. Your notes emphasize the importance of the root directory (/) and the root account as the place where everything begins—this same "start-of-the-chain" logic applies to your SSH keys

Backup the Configs: Just as your guide advises you to copy configuration files like /etc/ssh/sshd_config before you edit them, make sure you have a secure backup of your private key

If you lose that one key, you lose access to all your "batman" accounts across all your distros!
Tab for Accuracy: When copying your public key to your RHEL or Debian VMs, remember your "Check Engine light"—use the Tab key to ensure you are pointing to the correct key file in your .ssh directory

-------------------------------------------------------------------

# SSHing into Windows Environment
--------

## The Connection Command is the Same
From your Debian host, the command to start the session is identical to what you use for your RHEL VM: ssh username@<Windows_Server_IP> Your Debian terminal doesn't care what the remote OS is; it just needs an SSH service to talk to.
## The Environment is Different
The biggest change is what happens after you log in.
No Bash: As your sources point out, Linux defaults to the bash shell
. Windows Server will likely put you into PowerShell or CMD.
Different Commands: Commands you've been studying—like tail -f, grep, or df -h—will not work on a standard Windows command line because they are specific to the Linux/Unix environment

## SSH Key Transfer (ssh-copy-id usually fails)
This is where the "steps" differ most. On your Linux VMs, you used ssh-copy-id, which automatically places your public key in the /home/user/.ssh/authorized_keys file

Windows File Structure: Windows does not use the /home or /etc directory structure found in your Linux notes

Manual Setup: Because of this different structure, ssh-copy-id often fails when targeting Windows. You usually have to manually copy your public key and paste it into the authorized_keys file located in the Windows user's profile (typically C:\Users\Username\.ssh\authorized_keys).

## Administrator Permissions
--------
In your notes, you've learned that RHEL uses the wheel group for administrative tasks
. On Windows, the "Administrators" group handles these rights. If you are an admin on Windows, the SSH service often looks for a global authorized keys file (C:\ProgramData\ssh\administrators_authorized_keys) instead of the one in your user folder.
Senior Advice for your Notes:
Check the Port: Just as you use ss -tunlp (the "Tunnel-P" trick) to see if a Linux service is listening, you must ensure the OpenSSH Server feature is enabled and listening on the Windows Server side

Backups Still Apply: Even on Windows: editing the Windows SSH configuration file (sshd_config), copy it before you edit it
-------------------------------------------------------------------
# More about SSH and workflow
SSH login works in two phases:
Password auth : the "get your foot in the door" method
Key-based auth : the professional, secures way to actually use

## Basic Password Login
```bash
ssh username@ipAddress
```
Note : It'll prompt for the remote user's password. 
Works the same whether it's Debian or RHEL. 
SSH command doesn't care about the distro

## Specify a port (if the admin moved SSH off the default port 22):
```bash
ssh -p 2222 username@ipAddress
```

 ## Key-Based Auth (The Right Way)
 ### Generate your key (on your HOST machine)
 ```bash
 ssh-keygen -t ed25519 -C "tony-homelab"
```
Notes: 
-t ed25519 > use this algorithum. Modern, fast, and secure. Avoid RSA unless forced to.
-C > a comment/label to remember what the key is for
Hit Enter to accept the default path usually it's under this path(~/.ssh/id_ed25519) unless makes multiple keys
 ### Ship the public key to the remote VM
```bash
ssh-copy-id username@ipAddress
```
If you have multiple keys and need to specify which one:
```bash
ssh-copy-id -i ~/.ssh/id_debian.pub username@ipAddress
```
### Log in with no password
```bash
ssh username@ipAddress
```
### The SSH Config File - Recommended mode
``` bash
Host debian-vm
    HostName 192.168.1.50
    User tony
    IdentityFile ~/.ssh/id_debian

Host rhel-vm
    HostName 192.168.1.51
    User tony
    IdentityFile ~/.ssh/id_rhel
```
this intended for 
instead of typing 
```bash
ssh -i ~/.ssh/id_rhel tony@192.168.1.51
```
jus need to type 
```bash
ssh rhel-vm
```
### Verifying SSH is Running (Server Side)
# RHEL
```bash
systemctl status sshd
```
# Debian
```bash
systemctl status ssh
```
# Is it actually listening on port 22?
```bash
ss -tunlp | grep 22
```
### The permission problem, Common problem
```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
chmod 600 ~/.ssh/id_ed25519        # private key
chmod 644 ~/.ssh/id_ed25519.pub    # public key
```
On RHEL specifically, you may also hit SELinux blocking SSH if the .ssh dir was created in a incorect way. it can be fix with:
```bash
restorecon -Rv ~/.ssh
```
# Some differences in Debian and RH world 
# Debian vs RHEL — SSH Differences

| Thing | Debian | RHEL / Rocky / AlmaLinux |
|---|---|---|
| Default user (cloud/VM installs) | `debian` or  custom user | `ec2-user`, `rocky`, `alma`, or custom |
| SSH service name | `ssh` | `sshd` |
| Start/check SSH | `systemctl status ssh` | `systemctl status sshd` |
| Firewall | `ufw` (if installeds) | `firewalld` |
| allow SSH through firewall | `ufw allow ssh` | `firewall-cmd --add-service=ssh --permanent` then `firewall-cmd --reload` |


---------------------------------------------------------------
# check current RAM slots and speed
```bash
sudo dmidecode -t memory
```
This retrive:
How many RAM slots 
How many are occupied
 speed of the RAM runs at
Maximum supported RAM

-------------------------------------------------------------------
# Web Server and Firewall stuffs

```bash
sudo dnf/apt install nginx
systemctl start nginx
systemctl enable nginx
systemctl status nginx
```
notes: enable command is for boot. 
curl command is like a browser but in the terminal like vim/nano text editor but for terminals. it fetches a webpage and dumps the HTML to the screen.
## Check what firewalld is currently allowing
```bash
sudo firewall-cmd --list-all
```
### ports that need to open for web stuffs
Port 80 = http, regular web traffic
Port 443 = https, encrypted web traffics

note : on firewalld you don't open ports by number directly,  you open them by service names. more cleaner this ways. 

```bash
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload
```
--permanent flag means it survives a reboot. without this the rule disappears next boot/restart.
--reload applies the new rules without dropping existing connections.

verifying firewall rules saved correctly
```bash
sudo firewall-cmd --list-all
```
On RHEL nginx serves files from
```bash
/usr/share/nginx/html/
```
-> means symlink 
real nginx files live at 
```bash
/usr/share/testpage/index.html
```
The config that controls where nginx looks for files lives here >
```bash
/etc/nginx/nginx.conf
```
---------------
## Logs in Debian and RH 
```
/var/log/audit
```
note : audit logs are security related

### for general system activities on Debian is
```
Debian = /var/log/syslog
```

for general system activity on RHEL is
```
RHEL   = /var/log/messages
```





































































































