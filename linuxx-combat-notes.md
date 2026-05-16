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
```bash
cd /var/log
```
tree
Show directory structure visually
Use case: Understand project layout
Example:
```bash
tree /etc/nginx
```
clear
Clear terminal screen
Use case: Clean clutter
Example:
```bash 
clear
```

------------------------------------

# FILES AND DIRECTORIES

touch
Create empty file
Use case: Create placeholder/config quickly
Example:
```bash
touch test.txt
```
mkdir -p
Create nested directories
Use case: Build folder structure fast
Example:
```bash
mkdir -p /opt/projects/test
```
cp
Copy files/directories
Use case: Backup before editing
Example:
```bash
cp nginx.conf nginx.conf.bak
```
mv
Move or rename file
Use case: Rename configs/scripts
Example:
```bash
mv old.txt new.txt
```
rm -rf
Delete recursively
Use case: Remove junk folders
DANGER: Permanent delete
Example:
```bash
rm -rf testfolder/
```
find
Search for files
Use case: Locate missing configs/logs
Example:
```bash
find /etc -name "*.conf"
```
locate
Fast filename search
Use case: Quick searches
Example:
```bash
locate nginx.conf
```
# FILE VIEWING

cat
Print whole file
Use case: Small file viewing
Example:
```bash
cat /etc/os-release
```
less
Read long files safely
Use case: Logs/configs/manuals
Example:
```bash
less /var/log/syslog
```
tail -f
Watch live log updates
Use case: Real-time troubleshooting
Example:
```bash
tail -f /var/log/syslog
```
head
Show first lines
Use case: Quick preview
Example:
```bash
head -20 file.txt
```
------------------------------------

# TEXT SEARCHING

grep
Search text
Use case: Find errors/configs
Example:
```bash
grep "error" /var/log/syslog
```
grep -i
Case-insensitive search
Use case: Ignore capitalization
Example:
```bash
grep -i nginx file.txt
```
grep -r
Recursive search
Use case: Search entire directory
Example:
```bash
grep -r "listen 80" /etc/nginx
```
grep -v
Invert search
Use case: Exclude lines
Example:
```bash
grep -v "200" access.log
```
awk
Column-based text processing
Use case: Extract fields
Example:
```bash
awk '{print $1}' file.txt
```
sed
Stream editing
Use case: Replace text quickly
Example:
```bash
sed 's/nginx/apache/g' file.txt
```
------------------------------------

# SYSTEM INFORMATION

uname -a
Kernel/system info
Use case: Identify OS/kernel
Example:
```bash 
uname -a
```
hostnamectl
Show system identity
Use case: Check hostname/system version
Example:
```bash
hostnamectl
```
uptime
Show uptime/load
Use case: Check system stress
Example:
```bash
uptime
```
date
Show current date/time
Use case: Time verification
Example:
```bash
date
```
whoami
Current logged user
Use case: Verify privileges
Example:
```bash
whoami
```
id
Show UID/GID/groups
Use case: Permission troubleshooting
Example:
```bash
id
```
------------------------------------
# DISK AND STORAGE

df -h
Disk free space
Use case: Disk full investigation
Example:
```bash
df -h
```
du -sh
Directory size
Use case: Find space hogs
Example:
```bash
du -sh /var/log
```
lsblk
List disks/partitions
Use case: Storage inspection
Example:
```bash
lsblk
```
mount
Show mounted filesystems
Use case: Check mounted drives
Example:
```bash
mount
```

fdisk -l
Show partitions
Use case: Disk troubleshooting
Example:
```bash
sudo fdisk -l
```
------------------------------------

# PROCESS MANAGEMENT

ps aux
Show running processes
Use case: Process investigation
Example:
```bash
ps aux | grep nginx
```
top
Live system monitor
Use case: CPU/RAM troubleshooting
Example:
```bash
top
```
htop
Better interactive top
Use case: Easier monitoring
Example:
```bash
htop
```

kill
Terminate process politely
Use case: Stop stuck apps
Example:
```bash
kill 1234
```
kill -9
Force kill process
Use case: Frozen process
Example:
```bash
kill -9 1234
```
pkill
Kill by process name
Use case: Fast cleanup
Example:
```bash
pkill nginx
```
jobs
Show background jobs
Use case: Job control
Example:
```bash
jobs
```
------------------------------------

# SERVICES (SYSTEMD)

systemctl status
Check service status
Use case: Is service dead?
Example:
```bash
systemctl status nginx
```
systemctl restart
Restart service
Use case: Apply fixes/config changes
Example:
```bash
sudo systemctl restart nginx
```
systemctl start
Start service
Use case: Bring service online
Example:
```bash
sudo systemctl start nginx
```
systemctl stop
Stop service
Use case: Shutdown service safely
Example:
```bash
sudo systemctl stop nginx
```
systemctl enable
Start on boot
Use case: Persistent services
Example:
```bash
sudo systemctl enable nginx
```
journalctl -xe
View system logs
Use case: Investigate failures
Example:
```bash
journalctl -xe
```
journalctl -u
Service-specific logs
Use case: Debug service
Example:
```bash
journalctl -u nginx
```
------------------------------------

# NETWORKING

ip a
Show IP addresses
Use case: Network troubleshooting
Example:
```bash
ip a
```
ip r
Show routes/gateway
Use case: Routing issues
Example:
```bash
ip r
```
ping
Connectivity test
Use case: Network reachability
Example:
```bash
ping 8.8.8.8
```
ss -tunlp
Show listening ports
Use case: Check open services
Example:
```bash
ss -tunlp
```
curl
Test HTTP/API
Use case: Website/API testing
Example:
```bash
curl http://localhost
```
wget
Download files
Use case: Grab installers/files
Example:
```bash
wget https://example.com/file.txt
```
dig
DNS lookup
Use case: DNS troubleshooting
Example:
```bash
dig google.com
```
nslookup
Simple DNS check
Use case: Verify name resolution
Example:
```bash
nslookup google.com
```
traceroute
Trace network path
Use case: Network latency/routing
Example:
```bash
traceroute google.com
```
------------------------------------

# USERS AND PERMISSIONS

useradd
Create user
Use case: New accounts
Example:
```bash
sudo useradd morty
```
passwd
Set password
Use case: Account setup
Example:
```bash
sudo passwd morty
```
usermod -aG
Add user to group
Use case: Sudo/docker access
Example:
```bash
sudo usermod -aG wheel morty
```
chmod
Change permissions
Use case: Permission fixes
Example:
```bash
chmod 755 script.sh
```
chown
Change ownership
Use case: Service/file ownership
Example:
```bash
chown nginx:nginx file.txt
```
sudo
Run command as root
Use case: Administrative tasks
Example:
```bash
sudo dnf update
```
------------------------------------

# SSH

ssh
Remote login
Use case: Server management
Example:
```bash
ssh user@192.168.1.10
```
scp
Secure file copy
Use case: Transfer files
Example:
```bash
scp file.txt user@server:/tmp
```
ssh-keygen
Generate SSH keys
Use case: Passwordless login
Example:
```bash
ssh-keygen -t ed25519
```
PACKAGES

apt update
Refresh package list
Use case: Debian updates
Example:
```bash
sudo apt update
```
apt install
Install packages
Use case: Install software
Example:
```bash
sudo apt install htop
```
dnf install
Install packages (RHEL)
Use case: Rocky/RHEL package install
Example:
```bash
sudo dnf install htop
```
rpm -qa
List installed RPM packages
Use case: Package inspection
Example:
```bash
rpm -qa | grep nginx
```
------------------------------------

# ARCHIVES

tar -czvf
Compress archive
Use case: Backups
Example:
```bash
tar -czvf backup.tar.gz folder/
```
tar -xzvf
Extract archive
Use case: Restore files
Example:
```bash
tar -xzvf backup.tar.gz
```
------------------------------------

# LOGICAL OPERATORS

|
Pipe output
Use case: Chain commands
Example:
```bash
ps aux | grep nginx
```
&&
Run next command if success
Use case: Safe chaining
Example:
```bash
dnf update && reboot
```
&
Run in background
Use case: Long-running jobs
Example:
```bash
rsync backup/ remote/ &
``` 
# TROUBLESHOOTING FLOWS
## SERVICE NOT WORKING
```bash
systemctl status nginx
journalctl -u nginx
ss -tunlp
tail -f /var/log/nginx/error.log
```
## HIGH CPU
```bash
top
htop
ps aux --sort=-%cpu | head
```
## DISK FULL
```bash
df -h
du -sh /*
find /var/log -size +100M
```
## NETWORK ISSUE
```bash
ip a
ip r
ping 8.8.8.8
ping google.com
dig google.com
```
## SSH NOT WORKING
```bash
systemctl status sshd
ss -tunlp | grep 22
journalctl -u sshd
ping server-ip
```
## PERMISSION DENIED
```bash
ls -lah
id
chmod
chown
```
## WEB SERVER DOWN
```bash
systemctl status nginx
journalctl -u nginx
curl localhost
ss -tunlp | grep 80
```
------------------------------------

# ADVANCED REAL-WORLD COMMANDS
## FILE COMPARISON

diff
Compare files line-by-line
Use case: Config changes
Example:
```bash
diff old.conf new.conf
```
vimdiff
Visual file comparison
Use case: Debug config differences
Example:
```bash
vimdiff old.conf new.conf
```

------------------------------------

# NETWORK TROUBLESHOOTING

nc (netcat)
Test ports/connectivity
Use case: Check if port reachable
Example:
```bash
nc -zv 192.168.1.10 443
``` 
tcpdump
Capture network packets
Use case: Deep network troubleshooting
Example:
```bash
sudo tcpdump -i eth0 port 80
```
nmap
Port scanning
Use case: Discover services
Example:
```bash
nmap 192.168.1.10
```
arp -a
Show ARP table
Use case: Local network debugging
Example:
```bash
arp -a
```
------------------------------------

# SYSTEM PERFORMANCE

free -h
Check RAM/swap usage
Use case: Memory investigation
Example:
```bash
free -h
```
vmstat
System performance statistics
Use case: CPU/memory/io analysis
Example:
```bash
vmstat 1
```
iostat
Disk IO monitoring
Use case: Slow disk troubleshooting
Example:
```bash
iostat -xz 1
```
sar
Historical performance data
Use case: Investigate past spikes
Example:
```bash
sar -u 1 5
```
------------------------------------

# LOGS AND DEBUGGING

dmesg
Kernel/system boot logs
Use case: Hardware/kernel issues
Example:
```bash
dmesg | tail
``` 
watch
Run command repeatedly
Use case: Live monitoring
Example:
```bash
watch df -h
```
tee
Save and display output
Use case: Logging command output
Example:
```bash
ls -lah | tee output.txt
```
------------------------------------

# ARCHIVES AND FILE TRANSFER

rsync
Efficient sync/copy
Use case: Backups/deployments
Example:
```bash
rsync -av /source/ /backup/
```
zip
Compress files
Use case: Sharing archives
Example:
```bash
zip backup.zip file.txt
```
unzip
Extract zip archives
Use case: Restore/downloads
Example:
```bash
unzip backup.zip
```
------------------------------------

# PERMISSIONS AND OWNERSHIP

umask
Default file permissions
Use case: Security troubleshooting
Example:
```bash
umask
```
getfacl
View advanced ACL permissions
Use case: Complex permission issues
Example:
```bash
getfacl file.txt
```
setfacl
Set ACL permissions
Use case: Shared folder access
Example:
```bash
setfacl -m u:morty:rwx shared/
```
------------------------------------

# TEXT PROCESSING

cut
Extract columns
Use case: Parse logs/data
Example:
```bash
cut -d: -f1 /etc/passwd
```
sort
Sort lines
Use case: Organize output
Example:
```bash
sort file.txt
```
uniq
Remove duplicates
Use case: Analyze repeated data
Example:
```bash
sort file.txt | uniq
```
wc
Count lines/words/chars
Use case: Quick statistics
Example:
```bash
wc -l file.txt
```
xargs
Pass input as arguments
Use case: Bulk operations
Example:
```bash
find . -name "*.log" | xargs rm
```
tr
Translate characters
Use case: Formatting text
Example:
```bash
cat file.txt | tr 'a-z' 'A-Z'
```
------------------------------------

# DISK AND FILESYSTEM

blkid
Show UUID/filesystem info
Use case: Mount troubleshooting
Example:
```bash
blkid
```
fstab
Filesystem mount config
Use case: Persistent mounts
Example:
```bash
cat /etc/fstab
```
lsof
Show open files/process usage
Use case: "Why can't I delete this?"
Example:
```bash
lsof | grep deleted
```
------------------------------------

# JOBS AND BACKGROUND TASKS

nohup
Run command after logout
Use case: Long-running jobs
Example:
```bash
nohup backup.sh &
```
screen
Persistent terminal session
Use case: Remote admin sessions
Example:
```bash
screen
```
tmux
Modern terminal multiplexer
Use case: Multi-session remote work
Example:
```bash
tmux
```
------------------------------------

# SCHEDULING

crontab -e
Edit scheduled jobs
Use case: Automation
Example:
```bash
crontab -e
```
at
Run one-time future job
Use case: Delayed tasks
Example:
```bash
at now + 1 hour
```
------------------------------------

# LINUX ENGINEER SURVIVAL COMMANDS

history
Show command history
Use case: Remember previous commands
Example:
```bash
history
```
alias
Create command shortcuts
Use case: Faster workflow
Example:
```bash
alias ll='ls -lah'
```
env
Show environment variables
Use case: Debug environments
Example:
```bash
env
```
export
Set environment variable
Use case: Scripts/apps
Example:
```bash
export PATH=$PATH:/custom/bin
```
source
Reload shell config
Use case: Apply changes instantly
Example:
```bash
source ~/.bashrc
```
which
Locate executable
Use case: Verify command location
Example:
```bash
which python
```
whereis
Find binary/source/man page
Use case: Tool inspection
Example:
```bash
whereis nginx
```
------------------------------------

virtual machine trouble shooting. 
"Error starting domain: Requested operation is not valid: network 'default' is not active"
# 1. Start the default network
```bash
sudo virsh net-start default
```
# 2. Configure it to start automatically every time the PC boots
```bash
sudo virsh net-autostart default
```
# double-check that everything is configured correctly
```bash
sudo virsh net-list --all
```

If it complains about firewall/iptables or dnsmasq:
Libvirt needs dnsmasq for DHCP and an iptables backend to route traffic. 
Can fix this by installing the missing pieces based on the Linux distribution:

```bash
Ubuntu/Debian: sudo apt install dnsmasq iptables
Arch Linux: sudo pacman -S dnsmasq iptables-nft
Fedora/RHEL: sudo dnf install dnsmasq iptables-nft
```
#   Note: 
After installing, restart the service with sudo systemctl restart libvirtd and try the quick fix commands again.

If it says "network 'default' does not exist":
If the configuration file vanished entirely, it can be restore the factory default network profile with this command:
```bash
sudo virsh net-define /etc/libvirt/qemu/networks/default.xml
```