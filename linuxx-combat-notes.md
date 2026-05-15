# LINUX COMBAT TROUBLESHOOTING NOTES

# BASIC NAVIGATION

pwd
Show current directory
Use case: "Where the hell am I?"
Example:
pwd

ls -lah
List files with permissions and hidden files
Use case: Inspect folder quickly
Example:
ls -lah

cd
Change directory
Use case: Move around filesystem
Example:
cd /var/log

tree
Show directory structure visually
Use case: Understand project layout
Example:
tree /etc/nginx

clear
Clear terminal screen
Use case: Clean clutter
Example:
clear

------------------------------------

# FILES AND DIRECTORIES

touch
Create empty file
Use case: Create placeholder/config quickly
Example:
touch test.txt

mkdir -p
Create nested directories
Use case: Build folder structure fast
Example:
mkdir -p /opt/projects/test

cp
Copy files/directories
Use case: Backup before editing
Example:
cp nginx.conf nginx.conf.bak

mv
Move or rename file
Use case: Rename configs/scripts
Example:
mv old.txt new.txt

rm -rf
Delete recursively
Use case: Remove junk folders
DANGER: Permanent delete
Example:
rm -rf testfolder/

find
Search for files
Use case: Locate missing configs/logs
Example:
find /etc -name "*.conf"

locate
Fast filename search
Use case: Quick searches
Example:
locate nginx.conf

FILE VIEWING

cat
Print whole file
Use case: Small file viewing
Example:
cat /etc/os-release

less
Read long files safely
Use case: Logs/configs/manuals
Example:
less /var/log/syslog

tail -f
Watch live log updates
Use case: Real-time troubleshooting
Example:
tail -f /var/log/syslog

head
Show first lines
Use case: Quick preview
Example:
head -20 file.txt

------------------------------------

# TEXT SEARCHING

grep
Search text
Use case: Find errors/configs
Example:
grep "error" /var/log/syslog

grep -i
Case-insensitive search
Use case: Ignore capitalization
Example:
grep -i nginx file.txt

grep -r
Recursive search
Use case: Search entire directory
Example:
grep -r "listen 80" /etc/nginx

grep -v
Invert search
Use case: Exclude lines
Example:
grep -v "200" access.log

awk
Column-based text processing
Use case: Extract fields
Example:
awk '{print $1}' file.txt

sed
Stream editing
Use case: Replace text quickly
Example:
sed 's/nginx/apache/g' file.txt

------------------------------------

# SYSTEM INFORMATION

uname -a
Kernel/system info
Use case: Identify OS/kernel
Example:
uname -a

hostnamectl
Show system identity
Use case: Check hostname/system version
Example:
hostnamectl

uptime
Show uptime/load
Use case: Check system stress
Example:
uptime

date
Show current date/time
Use case: Time verification
Example:
date

whoami
Current logged user
Use case: Verify privileges
Example:
whoami

id
Show UID/GID/groups
Use case: Permission troubleshooting
Example:
id

------------------------------------
# DISK AND STORAGE

df -h
Disk free space
Use case: Disk full investigation
Example:
df -h

du -sh
Directory size
Use case: Find space hogs
Example:
du -sh /var/log

lsblk
List disks/partitions
Use case: Storage inspection
Example:
lsblk

mount
Show mounted filesystems
Use case: Check mounted drives
Example:
mount

fdisk -l
Show partitions
Use case: Disk troubleshooting
Example:
sudo fdisk -l

------------------------------------

# PROCESS MANAGEMENT

ps aux
Show running processes
Use case: Process investigation
Example:
ps aux | grep nginx

top
Live system monitor
Use case: CPU/RAM troubleshooting
Example:
top

htop
Better interactive top
Use case: Easier monitoring
Example:
htop

kill
Terminate process politely
Use case: Stop stuck apps
Example:
kill 1234

kill -9
Force kill process
Use case: Frozen process
Example:
kill -9 1234

pkill
Kill by process name
Use case: Fast cleanup
Example:
pkill nginx

jobs
Show background jobs
Use case: Job control
Example:
jobs

------------------------------------

# SERVICES (SYSTEMD)

systemctl status
Check service status
Use case: Is service dead?
Example:
systemctl status nginx

systemctl restart
Restart service
Use case: Apply fixes/config changes
Example:
sudo systemctl restart nginx

systemctl start
Start service
Use case: Bring service online
Example:
sudo systemctl start nginx

systemctl stop
Stop service
Use case: Shutdown service safely
Example:
sudo systemctl stop nginx

systemctl enable
Start on boot
Use case: Persistent services
Example:
sudo systemctl enable nginx

journalctl -xe
View system logs
Use case: Investigate failures
Example:
journalctl -xe

journalctl -u
Service-specific logs
Use case: Debug service
Example:
journalctl -u nginx

------------------------------------

# NETWORKING

ip a
Show IP addresses
Use case: Network troubleshooting
Example:
ip a

ip r
Show routes/gateway
Use case: Routing issues
Example:
ip r

ping
Connectivity test
Use case: Network reachability
Example:
ping 8.8.8.8

ss -tunlp
Show listening ports
Use case: Check open services
Example:
ss -tunlp

curl
Test HTTP/API
Use case: Website/API testing
Example:
curl http://localhost

wget
Download files
Use case: Grab installers/files
Example:
wget https://example.com/file.txt

dig
DNS lookup
Use case: DNS troubleshooting
Example:
dig google.com

nslookup
Simple DNS check
Use case: Verify name resolution
Example:
nslookup google.com

traceroute
Trace network path
Use case: Network latency/routing
Example:
traceroute google.com

------------------------------------

# USERS AND PERMISSIONS

useradd
Create user
Use case: New accounts
Example:
sudo useradd morty

passwd
Set password
Use case: Account setup
Example:
sudo passwd morty

usermod -aG
Add user to group
Use case: Sudo/docker access
Example:
sudo usermod -aG wheel morty

chmod
Change permissions
Use case: Permission fixes
Example:
chmod 755 script.sh

chown
Change ownership
Use case: Service/file ownership
Example:
chown nginx:nginx file.txt

sudo
Run command as root
Use case: Administrative tasks
Example:
sudo dnf update

------------------------------------

# SSH

ssh
Remote login
Use case: Server management
Example:
ssh user@192.168.1.10

scp
Secure file copy
Use case: Transfer files
Example:
scp file.txt user@server:/tmp

ssh-keygen
Generate SSH keys
Use case: Passwordless login
Example:
ssh-keygen -t ed25519

PACKAGES

apt update
Refresh package list
Use case: Debian updates
Example:
sudo apt update

apt install
Install packages
Use case: Install software
Example:
sudo apt install htop

dnf install
Install packages (RHEL)
Use case: Rocky/RHEL package install
Example:
sudo dnf install htop

rpm -qa
List installed RPM packages
Use case: Package inspection
Example:
rpm -qa | grep nginx
------------------------------------

# ARCHIVES

tar -czvf
Compress archive
Use case: Backups
Example:
tar -czvf backup.tar.gz folder/

tar -xzvf
Extract archive
Use case: Restore files
Example:
tar -xzvf backup.tar.gz

------------------------------------

# LOGICAL OPERATORS

|
Pipe output
Use case: Chain commands
Example:
ps aux | grep nginx

&&
Run next command if success
Use case: Safe chaining
Example:
dnf update && reboot

&
Run in background
Use case: Long-running jobs
Example:
rsync backup/ remote/ &

# TROUBLESHOOTING FLOWS
## SERVICE NOT WORKING

systemctl status nginx
journalctl -u nginx
ss -tunlp
tail -f /var/log/nginx/error.log

## HIGH CPU

top
htop
ps aux --sort=-%cpu | head

## DISK FULL

df -h
du -sh /*
find /var/log -size +100M

## NETWORK ISSUE

ip a
ip r
ping 8.8.8.8
ping google.com
dig google.com

## SSH NOT WORKING

systemctl status sshd
ss -tunlp | grep 22
journalctl -u sshd
ping server-ip

## PERMISSION DENIED

ls -lah
id
chmod
chown

## WEB SERVER DOWN

systemctl status nginx
journalctl -u nginx
curl localhost
ss -tunlp | grep 80

------------------------------------

# ADVANCED REAL-WORLD COMMANDS
## FILE COMPARISON

diff
Compare files line-by-line
Use case: Config changes
Example:
diff old.conf new.conf

vimdiff
Visual file comparison
Use case: Debug config differences
Example:
vimdiff old.conf new.conf


------------------------------------

# NETWORK TROUBLESHOOTING

nc (netcat)
Test ports/connectivity
Use case: Check if port reachable
Example:
nc -zv 192.168.1.10 443

tcpdump
Capture network packets
Use case: Deep network troubleshooting
Example:
sudo tcpdump -i eth0 port 80

nmap
Port scanning
Use case: Discover services
Example:
nmap 192.168.1.10

arp -a
Show ARP table
Use case: Local network debugging
Example:
arp -a

------------------------------------

# SYSTEM PERFORMANCE

free -h
Check RAM/swap usage
Use case: Memory investigation
Example:
free -h

vmstat
System performance statistics
Use case: CPU/memory/io analysis
Example:
vmstat 1

iostat
Disk IO monitoring
Use case: Slow disk troubleshooting
Example:
iostat -xz 1

sar
Historical performance data
Use case: Investigate past spikes
Example:
sar -u 1 5

------------------------------------

# LOGS AND DEBUGGING

dmesg
Kernel/system boot logs
Use case: Hardware/kernel issues
Example:
dmesg | tail

watch
Run command repeatedly
Use case: Live monitoring
Example:
watch df -h

tee
Save and display output
Use case: Logging command output
Example:
ls -lah | tee output.txt

------------------------------------

# ARCHIVES AND FILE TRANSFER

rsync
Efficient sync/copy
Use case: Backups/deployments
Example:
rsync -av /source/ /backup/

zip
Compress files
Use case: Sharing archives
Example:
zip backup.zip file.txt

unzip
Extract zip archives
Use case: Restore/downloads
Example:
unzip backup.zip

------------------------------------

# PERMISSIONS AND OWNERSHIP

umask
Default file permissions
Use case: Security troubleshooting
Example:
umask

getfacl
View advanced ACL permissions
Use case: Complex permission issues
Example:
getfacl file.txt

setfacl
Set ACL permissions
Use case: Shared folder access
Example:
setfacl -m u:morty:rwx shared/

------------------------------------

# TEXT PROCESSING

cut
Extract columns
Use case: Parse logs/data
Example:
cut -d: -f1 /etc/passwd

sort
Sort lines
Use case: Organize output
Example:
sort file.txt

uniq
Remove duplicates
Use case: Analyze repeated data
Example:
sort file.txt | uniq

wc
Count lines/words/chars
Use case: Quick statistics
Example:
wc -l file.txt

xargs
Pass input as arguments
Use case: Bulk operations
Example:
find . -name "*.log" | xargs rm

tr
Translate characters
Use case: Formatting text
Example:
cat file.txt | tr 'a-z' 'A-Z'

------------------------------------

# DISK AND FILESYSTEM

blkid
Show UUID/filesystem info
Use case: Mount troubleshooting
Example:
blkid

fstab
Filesystem mount config
Use case: Persistent mounts
Example:
cat /etc/fstab

lsof
Show open files/process usage
Use case: "Why can't I delete this?"
Example:
lsof | grep deleted

------------------------------------

# JOBS AND BACKGROUND TASKS

nohup
Run command after logout
Use case: Long-running jobs
Example:
nohup backup.sh &

screen
Persistent terminal session
Use case: Remote admin sessions
Example:
screen

tmux
Modern terminal multiplexer
Use case: Multi-session remote work
Example:
tmux

------------------------------------

# SCHEDULING

crontab -e
Edit scheduled jobs
Use case: Automation
Example:
crontab -e

at
Run one-time future job
Use case: Delayed tasks
Example:
at now + 1 hour

------------------------------------

# LINUX ENGINEER SURVIVAL COMMANDS

history
Show command history
Use case: Remember previous commands
Example:
history

alias
Create command shortcuts
Use case: Faster workflow
Example:
alias ll='ls -lah'

env
Show environment variables
Use case: Debug environments
Example:
env

export
Set environment variable
Use case: Scripts/apps
Example:
export PATH=$PATH:/custom/bin

source
Reload shell config
Use case: Apply changes instantly
Example:
source ~/.bashrc

which
Locate executable
Use case: Verify command location
Example:
which python

whereis
Find binary/source/man page
Use case: Tool inspection
Example:
whereis nginx

------------------------------------