# Linux General Notes

The act of one process giving up control of the CPU to another process is called a context switch. 
---------------------------------------------------------
# User Space
the main memory that the kernel allocates for user processes is called user space. 
Because a process is simply a state (or image) in memory, user space also refers to the memory for the entire collection of running processes. 
(You may also hear the more informal term userland used for user space; sometimes this also means the programs running in user space.) 
Most of the real action on a Linux system happens in user space. 
---------------------------------------------------------
# The Bourne Shell: /bin/sh
There are many different Unix shells, but all derive features from the Bourne shell (/bin/sh), a standard shell developed at Bell Labs for early versions of Unix. 
Every Unix system needs a version of the Bourne shell in order to function correctly. 
Linux uses an enhanced version of the Bourne shell called bash or the “Bourne-again” shell. 
The bash shell is the default shell on most Linux distributions, and /bin/sh is normally a link to bash on a Linux system. 

You may not have bash as your default shell if you’re using this chapter as a guide
for a Unix account at an organization where you’re not the system administrator. You
can change your shell with chsh or ask your system administrator for help. 
The best practice when running them is to use sudo in order to provide some protection and a log that you can look up later for possible errors.

Pressing CTRL-D on an empty line stops the current standard input entry from the terminal with an EOF (end-of-file) message (and often terminates a program). Don’t confuse this with CTRL-C, which usually terminates a program regardless of its input or output.
---------------------------------------------------------
# Navigating Directories
The Unix directory hierarchy starts at /, also called the root directory. 
The directory separator is the slash (/), not the backslash (\). There are several Basic Commands and Directory Hierarchy standard subdirectories in the root directory, such as /usr. 
When you refer to a file or directory, you specify a path or pathname. When a path starts with / (such as /usr/lib), it’s a full or absolute path.

A path component identified by two dots (..) specifies the parent of a directory. For example, if you’re working in /usr/lib, the path .. would refer to /usr. 
Similarly, ../bin would refer to /usr/bin. One dot (.) refers to the current directory; for example, if you’re in /usr/lib, the path . is still /usr/lib, and ./X11 is /usr/lib/X11. You won’t have to use . very often because most commands default to the current directory if a path doesn’t start with / (so you could just use X11 instead of ./X11 in the preceding example). 
A path not beginning with / is called a relative path. 
Most of the time, you’ll work with relative pathnames, because you’ll already be in or near the directory you need.

If you’re used to the Windows command prompt, you might instinctively type *.* to match all files. 
Break this habit now. In Linux and other versions of Unix, you must use * to match all files. In the Unix shell, *.* matches only files and directories that contain the dot (.) character in their names. 
Unix filenames do not need extensions and often do not carry them.
Another shell glob character, the question mark (?), instructs the shell to match exactly one arbitrary character. 
For example, b?at matches boat and brat. If you don’t want the shell to expand a glob in a command, enclose the glob in single quotes (''). For example, the command echo '*' prints a star. 
---------------------------------------------------------
# Permission
This is called an absolute change because it sets all permission bits at once. To understand how this works, you need to know how to represent the permission bits in octal form (each numeral represents a number in base 8, 0 through 7, and corresponds to a permission set). See the chmod(1) manual page or info manual for more. You don’t really need to know how to construct absolute modes if you prefer to use them; just memorize the modes that you use most often.

Table 2-4 lists the most common ones 
You may sometimes see people changing permissions with numbers, for
example: $ chmod 644 file
644 user: read/write; group, other: read files
600 user: read/write; group, other: none files
755 user: read/write/execute; group, other: read/execute directories, programs
700 user: read/write/execute; group, other: none directories, programs
711 user: read/write/execute; group, other: execute directories
---------------------------------------------------------
# Hard Disks: /dev/sd*
Most hard disks attached to current Linux systems correspond to device names with an sd prefix, such as /dev/sda, /dev/sdb, and so on. 
These devices represent entire disks; the kernel makes separate device files, such as /dev/ sda1 and /dev/sda2, for the partitions on a disk. The naming convention requires a little explanation. 
The sd portion of the name stands for SCSI disk. Small Computer System Interface (SCSI) was originally developed as a hardware and protocol standard for communication between devices such as disks and other peripherals. 
To list the SCSI devices on your system, use a utility that walks the device paths provided by sysfs. 
One of the most succinct tools is lsscsi.  
Linux assigns devices to device files in the order in which its drivers
encounter the devices. 
So, in the previous example, the kernel found the disk first and the flash drive second. 
Unfortunately, this device assignment scheme has traditionally caused problems when you are reconfiguring hardware. 
Say, for example, that you have a system with three disks: /dev/sda, /dev/sdb, and /dev/sdc. 
If /dev/sdb explodes and you must remove it so that the machine can work again, the former /dev/sdc moves to /dev/sdb, and there’s no longer a /dev/sdc. If you were referring to the device names directly in the fstab file (see Section 4.2.8), you’d have to make some changes to that file in order to get things (mostly) back to normal. 
To solve this problem, many Linux systems use the Universally Unique Identifier (UUID; see Section 4.2.4) and/or the Logical Volume Manager (LVM) stable disk device mapping.
---------------------------------------------------------
# Absolute pathname
An absolute pathname begins with the root directory (/) and follows the tree, branch by branch, until it reaches the desired directory or file. Absolute paths always start with /.

# Relative pathname
A relative pathname starts from the present working directory. Relative paths never start with /.

Multiple slashes (/) between directories and files are allowed, but all but one slash between elements in the pathname is ignored by the system. While ////usr//bin is valid, it is seen as just /usr/bin by the system.
Most of the time, it is most convenient to use relative paths, which require less typing. 
Usually, you take advantage of the shortcuts provided by: . (present directory), .. (parent directory) and ~ (your home directory).
For example, suppose you are currently working in your home directory and wish to move to the /usr/bin directory.
The following two ways will bring you to the same directory from your home directory:

## Absolute pathname method
$ cd /usr/bin
## Relative pathname method
$ cd ../../usr/bin
---------------------------------------------------------
# Working with Files
## touch
touch is often used to set or update the access, change, and modify times of files. 
By default, it resets a file's timestamp to match the current time.
However, you can also create an empty file using touch:
$ touch <filename>
This is normally done to create an empty file as a placeholder for a later purpose.
touch provides several useful options. 
For example, the -t option allows you to set the date and timestamp of the file to a specific value, as in:
$ touch -t 12091600 myfile
This sets the myfile file's timestamp to 4 p.m., December 9th (12 09 1600).

mkdir sampdir 
It creates a sample directory named sampdir under the current directory. 
mkdir /usr/sampdir 
It creates a sample directory called sampdir under /usr.
Removing a directory is done with rmdir. 
The directory must be empty or the command will fail. 
To remove a directory and all of its contents you have to do rm -rf.

Moving, Renaming or Removing a File 
mv =		 Rename a file / move a file
rm = 	Remove a file
rm -f =	 Forcefully remove a file
rm -i	= 	Interactively remove a file
rmdir =	Remove an empty directory
rm -rf =	Forcefully remove a directory recursively
---------------------------------------------------------
standard input	= stdin = value	0	= example (keyboard)
standard output = stdout = value	1	= example (terminal)
standard error	=  stderr	= value 2 = example (log file)

## I/O Redirection
Through the command shell, we can redirect the three standard file streams so that we can get input from either a file or another command, instead of from our keyboard, and we can write output and errors to files or use them to provide input for subsequent commands.

For example, if we have a program called do_something that reads from stdin and writes to stdout and stderr, we can change its input source by using the less-than sign (<) followed by the name of the file to be consumed for input data:
$ do_something < input-file
If you want to send the output to a file, use the greater-than sign (>) as in:
$ do_something > output-file
In fact, you can do both at the same time as in:
$ do_something < input-file > output-file

Because stderr is not the same as stdout, error messages will still be seen on the terminal windows in the above example.

If you want to redirect stderr to a separate file, you use stderr’s file descriptor number, the greater-than sign, followed by the name of the file you want to receive everything the running command writes to stderr:
$ do_something 2> error-file
NOTE: By the same logic, do_something 1> output-file is the same as do_something > output-file.
A special shorthand notation can send anything written to file descriptor 2 (stderr) to the same place as file descriptor 1 (stdout): 2>&1.
$ do_something > all-output-file 2>&1
bash permits an easier syntax for the above:
$ do_something >& all-output-file
---------------------------------------------------------
# Pipes
The UNIX/Linux philosophy is to have many simple and short programs (or commands) cooperate together to produce quite complex results, rather than have one complex program with many possible options and modes of operation. 
In order to accomplish this, extensive use of pipes is made. You can pipe the output of one command or program into another as its input.
In order to do this, we use the vertical-bar, pipe symbol (|), between commands as in:
 
$ command1 | command2 | command3

The above represents what we often call a pipeline, and allows Linux to combine the actions of several commands into one. 
This is extraordinarily efficient because command2 and command3 do not have to wait for the previous pipeline commands to complete before they can begin processing at the data in their input streams; on multiple CPU or core systems, the available computing power is much better utilized and things get done quicker.
Furthermore, there is no need to save output in (temporary) files between the stages in the pipeline, which saves disk space and reduces reading and writing from disk, which often constitutes the slowest bottleneck in getting something done.

---------------------------------------------------------
# Searching for Files - `locate`, `find`, `whereis`, `which`

The main tools for doing this are the `locate` and `find` utilities.

| Command   | Searches              | Speed   | Use Case                          |
|-----------|-----------------------|---------|-----------------------------------|
| `find`    | Live filesystem       | Slow    | Flexible, powerful, real-time     |
| `locate`  | Pre-built database    | Fast    | Quick search, okay if slightly outdated |
| `which`   | PATH only             | Instant | "Where is this command?"          |
| `whereis` | System dirs only      | Instant | Binary + man page location        |


locate
The locate utility program performs a search while taking advantage of a previously constructed database of files and directories on your system, matching all entries that contain a specified character string. This can sometimes result in a very long list.

To get a shorter (and possibly more relevant) list, we can use the grep program as a filter. grep will print only the lines that contain one or more specified strings, as in: 
$ locate zip | grep bin

which will list all the files and directories with both zip and bin in their name.
Notice the use of | to pipe the two commands together.
locate utilizes a database created by a related utility, updatedb. Most Linux systems run this automatically once a day. 
However, you can update it at any time by just running updatedb from the command line as the root user.
---------------------------------------------------------
# Wildcards and Matching Filenames
You can search for a filename containing specific characters using wildcards.
Table: Wildcards
Wildcard	Result
?	Matches any single character
*	Matches any string of characters
[set]	Matches any character in the set of characters, for example [adf] will match any occurrence of a, d, or f
[!set]	Matches any character not in the set of characters

To search for files using the ? wildcard, replace each unknown character with ?. 
For example, if you know only the first two letters are 'ba' of a three-letter filename with an extension of .out, type ls ba?.out.

To search for files using the * wildcard, replace the unknown string with *. For example, if you remember only that the extension was .out, type ls *.out.

---------------------------------------------------------

# The find Program
find is an extremely useful and often-used utility program in the daily life of a Linux system administrator. 
It recurses down the filesystem tree from any particular directory (or set of directories) and locates files that match specified conditions. The default pathname is always the present working directory.

For example, administrators sometimes scan for potentially large core files (which contain diagnostic information after a program fails) that are more than several weeks old in order to remove them.

It is also common to remove files non-essential or outdated files in /tmp (and other volatile directories, such as those under /var/cache/ containing dispensable cached files) that have not been accessed recently.
Many Linux distributions use shell scripts that run periodically (through cron usually) to perform such house cleaning.

## Using find
When no arguments are given, find lists all files in the current directory and all of its subdirectories. Commonly used options to shorten the list include -name (only list files with a certain pattern in their name), -iname (also ignore the case of file names), and -type (which will restrict the results to files of a certain specified type, such as d for directory, l for symbolic link, or f for a regular file, etc.). 
Searching for files and directories named gcc:
$ find /usr -name gcc
Searching only for directories named gcc:
$ find /usr -type d -name gcc
Searching only for regular files named gcc:
$ find /usr -type f -name gcc

## Using Advanced find Options
Another good use of find is being able to run commands on the files that match your search criteria. The -exec option is used for this purpose.
To find and remove all files that end with .swp:

$ find -name "*.swp" -exec rm {} ';'      
[this command is for Finding and Removing Files that End with .swp]

The {} (squiggly brackets) is a placeholder that will be filled with all the file names that result from the find expression, and the preceding command will be run on each one individually.
Please note that you have to end the command with either ';' (including the single-quotes) or \;. Both forms are fine.
One can also use the -ok option, which behaves the same as -exec, except that find will prompt you for permission before executing the command. This makes it a good way to test your results before blindly executing any potentially dangerous commands. 

Finding Files Based on Time and Size
It is sometimes the case that you wish to find files according to attributes, such as when they were created, last used, etc., or based on their size. It is easy to perform such searches.
To find files based on time:

$ find / -ctime 3

Here, -ctime is when the inode metadata (i.e. file ownership, permissions, etc.) last changed; it is often, but not necessarily, when the file was first created. 
You can also search for accessed/last read (-atime) or 
modified/last written (-mtime) times. 
The number is the number of days and can be expressed as either a number (n) that means exactly that value, +n, which means greater than that number, or -n, which means less than that number. 
There are similar options for times in minutes (as in -cmin, -amin, and -mmin).

To find files based on sizes:

$ find / -size 0

Note the size here is in 512-byte blocks, by default; you can also specify bytes (c), kilobytes (k), megabytes (M), gigabytes (G), etc. 
As with the time numbers above, file sizes can also be exact numbers (n), +n or -n. For details, consult the man page for find.
For example, to find files greater than 10 MB in size and running a command on those files:

$ find / -size +10M -exec command {} ’;’
---------------------------------------------------------
# Package Management Systems on Linux
The core parts of a Linux distribution and most of its add-on software are installed via the Package Management System. 
There are two broad families of package managers widely deployed: those based on Debian and those which use RPM as their low-level package manager.  
Both package management systems operate on two distinct levels: a low-level tool (such as dpkg or rpm) takes care of the details of unpacking individual packages, running scripts, getting the software installed correctly, while a high-level tool (such as apt, dnf, or zypper) works with groups of packages, downloads packages from the vendor, and figures out dependencies. 
Most of the time users need to work only with the high-level tool, which will take care of calling the low-level tool as needed. 
Dependency resolution is a particularly important feature of the high-level tool, as it handles the details of finding and installing each dependency for you. 
Be careful, however, as installing a single package could result in many dozens or even hundreds of dependent packages being installed.

Debian - APT - dpkg - Linux System (APT stands for Advanced Packaging Tool)
Suse family system - zypper - rpm - Linux System
Red Hat system - dnf - rpm - Linux System

The Advanced Packaging Tool (apt) is the underlying package management system that manages software on Debian-based systems.  
While it forms the backend for graphical package managers, such as the Ubuntu Software Center and synaptic, its native user interface is at the command line, with programs that include apt (or apt-get) and apt-cache. 
dnf is the open source command-line package-management utility for the RPM-compatible Linux systems that belong to the Red Hat family.

# Basic Packaging Commands

| Operation                        | RPM                              | Debian                        |
|----------------------------------|----------------------------------|-------------------------------|
| Install package                  | `rpm -i foo.rpm`                 | `dpkg --install foo.deb`      |
| Install package + dependencies   | `dnf install foo`                | `apt install foo`             |
| Remove package                   | `rpm -e foo.rpm`                 | `dpkg --remove foo.deb`       |
| Remove package + dependencies    | `dnf remove foo`                 | `apt autoremove foo`          |
| Update package                   | `rpm -U foo.rpm`                 | `dpkg --install foo.deb`      |
| Update package + dependencies    | `dnf update foo`                 | `apt install foo`             |
| Update entire system             | `dnf update`                     | `apt dist-upgrade`            |
| Show all installed packages      | `rpm -qa` or `dnf list installed`| `dpkg --list`                 |
| Get information on package       | `rpm -qil foo`                   | `dpkg --listfiles foo`        |
| Show packages named foo          | `dnf list "foo"`                 | `apt-cache search foo`        |
| Show all available packages      | `dnf list`                       | `apt-cache dumpavail foo`     |
| What package is a file part of?  | `rpm -qf file`                   | `dpkg --search file`          |
---------------------------------------------------------
# Linux Documentation Sources
## The man pages (the man command)
---------------------------------------------------------
The man pages are the most often-used source of Linux documentation. They provide in-depth documentation about many programs and utilities, as well as other topics, including configuration files and programming APIs for system calls, library routines, and the kernel.
They are present on all Linux distributions and are always at your fingertips. 
The name man is just an abbreviation for manual. The man program searches, formats, and displays the information contained in the man page system.
A given topic may have multiple pages associated with it and there is a default order determining which one is displayed when no options or section number is specified. 
To list all pages on the topic, use the -f option. To list all pages that discuss a specific topic (even if the specified subject is not present in the name), use the –k option.

man –f (generates the same result as typing whatis.)
man –k (generates the same result as typing apropos.)

The man pages are divided into chapters numbered 1 through 9. In some cases, a letter is appended to the chapter number to identify a specific topic. 
For example, many pages describing part of the X Window API are in chapter 3X.
The chapter number can be used to force man to display the page from a particular chapter. 
It is common to have multiple pages across multiple chapters with the same name, especially for names of library functions or system calls.
With the -a parameter, man will display all pages with the given name in all chapters, one after the other, as in:

$ man -a socket

---------------------------------------------------------
# What Is a Process?
A process is simply an instance of one or more related tasks (threads) executing on your computer. 
It is not the same as a program or a command. 
A single command may actually start several processes simultaneously. Some processes are independent of each other and others are related. 
A failure of one process may or may not affect the others running on the system.
Processes - Processes use many system resources, such as memory, CPU (central processing unit) cycles, and peripheral devices, such as network cards, hard drives, printers, and displays. 
The operating system (especially the kernel) is responsible for allocating a proper share of these resources to each process and ensuring overall optimized system utilization.

# Process Types

| Process Type         | Description                                                                                                                                                                                                                                                          | Examples                                    |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------|
| Interactive          | Need to be started by a user, either at a command line or through a graphical interface such as an icon or a menu selection.                                                                                                                                         | `bash`, `firefox`, `top`, `slack`, `libreoffice` |
| Batch                | Automatic processes which are scheduled from and then disconnected from the terminal. These tasks are queued and work on a FIFO (First-In, First-Out) basis.                                                                                                         | `updatedb`, `ldconfig`                      |
| Daemons              | Server processes that run continuously. Many are launched during system startup and then wait for a user or system request indicating that their service is required.                                                                                                 | `httpd`, `sshd`, `libvirtd`, `cupsd`        |
| Threads              | Lightweight processes that run under the umbrella of a main process, sharing memory and other resources, but are scheduled and run by the system on an individual basis. A thread can end without terminating the whole process, and a process can create new threads at any time. Many non-trivial programs are multi-threaded. | `dconf-service`, `gnome-terminal-server`    |
| Kernel Threads       | Kernel tasks that users neither start nor terminate and have little control over. These may perform actions like moving a thread from one CPU to another, or ensuring that I/O operations to disk are completed.                                                      | `kthreadd`, `migration`, `ksoftirqd`        |
-------------------------------------------------------------------
# Process Scheduling and States

A critical kernel function called the scheduler constantly shifts processes on and off the CPU, sharing time according to relative priority, how much time is needed and how much has already been granted to a task. 
When a process is in a so-called running state,
 it means it is either currently executing instructions on a CPU, or is waiting to be granted a share of time (a time slice) so it can execute. All processes in this state reside on what is called a run queue and on a computer with multiple CPUs, or cores,
 there is a run queue on each. However, sometimes processes go into what is called a sleep state, generally when they are waiting for something to happen before they can resume, 
perhaps for the user to type something. In this condition, a process is said to be sitting in a wait queue. 
There are some other less frequent process states, especially when a process is terminating. Sometimes, a child process completes, 
but its parent process has not asked about its state. 
Amusingly, such a process is said to be in a zombie state; it is not really alive, but still shows up in the system's list of processes.
# Process and Thread IDs

| ID Type                    | Description                                                                                                                                                                                          |
|----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Process ID (PID)           | Unique Process ID number.                                                                                                                                                                            |
| Parent Process ID (PPID)   | The process (parent) that started this process. If the parent dies, the PPID will refer to an adoptive parent; on modern kernels, this is `kthreadd` which has PPID=2.                              |
| Thread ID (TID)            | Thread ID number. This is the same as the PID for single-threaded processes. For a multi-threaded process, each thread shares the same PID, but has a unique TID.                                   |

## Terminating a Process

At some point, one of your applications may stop working properly. How do you eliminate it?
-SIGKILL <pid> or kill -9 <pid>
Note, however, you can only kill your own processes; those belonging to another user are off-limits unless you are root (the name kill is historical and somewhat misleading; 
the command can be used to send any kind of signal to a process, not just a termination one).

# User and Group IDs
Many users can access a system simultaneously, and each user can run multiple processes. 
The operating system identifies the user who starts the process by the Real User ID (RUID) assigned to the user.
The user who determines the access rights for the users is identified by the Effective UID (EUID). 
The EUID may or may not be the same as the RUID.
Users can be organized into enumerated groups. 
Each group is identified by the Real Group ID (RGID). 
The access rights of the group are determined by the Effective Group ID (EGID). 
Each user can be a member of one or more groups.
** Most of the time we ignore these details and just talk about the User ID (UID) and Group ID (GID). **
User ID - RUID - Identifies the user who started the process
User Group ID - RGID - Identifies the group that started the process
EUID - Determines the access rights of the user 
EGID -  Determines the access rights of the group

## More About Priorities
At any given time, many processes are running (i.e. in the run queue) on the system. However, a CPU can actually accommodate only one task at a time, just like a car can have only one driver at a time. 
Some processes are more important than others, 
so Linux allows you to set and manipulate process priority. 
Higher priority processes get preferential access to the CPU. 
The priority for a process can be set by specifying a nice value, or niceness, for the process. 

The lower the nice value, the higher the priority. Low values are assigned to important processes, while high values are assigned to processes that can wait longer. 
A process with a high nice value simply allows other processes to be executed first.
In Linux, a nice value of -20 represents the highest priority and +19 represents the lowest.

nice Output 

cat nice.out
You can also assign a so-called real-time priority to time-sensitive tasks, such as controlling machines through a computer or collecting incoming data.

This is just a very high priority and is not to be confused with what is called hard real-time, which is conceptually different and has more to do with making sure a job gets completed within a very well-defined time window.

## Load Averages
The load average is the average of the load number for a given period of time. It takes into account processes that are:

Actively running on a CPU.
Considered runnable, but waiting on the run queue for a CPU to become available.
Sleeping: i.e. waiting for some kind of resource (typically, I/O) to become available.

NOTE: Linux differs from other UNIX-like operating systems in that it includes the sleeping processes. 
Furthermore, it only includes so-called uninterruptible sleepers, those which cannot be awakened easily.

The load average can be viewed by running w, top or uptime. 

Interpreting Load Averages
The load average is displayed using three numbers (0.45, 0.17, and 0.12) in the below screenshot. 
Assuming our system is a single-CPU system, the three load average numbers are interpreted as follows:

0.45: For the last minute the system has been 45% utilized on average.
0.17: For the last 5 minutes utilization has been 17%.
0.12: For the last 15 minutes utilization has been 12%.

If we saw a value of 1.00 in the second position, that would imply that the single-CPU system was 100% utilized, on average, over the past 5 minutes; this is good if we want to fully use a system. 

A value over 1.00 for a single-CPU system implies that the system was over-utilized: there were more processes needing CPU than CPU was available.

If we had more than one CPU, say a quad-CPU system, we would divide the load average numbers by the number of CPUs.

 In this case, for example, seeing a 1 minute load average of 4.00 implies that the system as a whole was 100% (4.00/4) utilized during the last minute.

Short-term increases are usually not a problem. 
A high peak you see is likely a burst of activity, not a new level. 
For example, at start up, many processes start and then activity settles down. 
If a high peak is seen in the 5 and 15 minute load averages, it may be cause for concern.

---------------------------------------------------------
# Background and Foreground Processes
Linux supports background and foreground job processing. A job in this context is just a command launched from a terminal window. 

Foreground jobs run directly from the shell, and when one foreground job is running, other jobs need to wait for shell access (at least in that terminal window if using the GUI) until it is completed.
This is fine when jobs complete quickly. 
But this can have an adverse effect if the current job is going to take a long time (even several hours) to complete.

In such cases, you can run the job in the background and free the shell for other tasks. 

The background job will be executed at lower priority, which, in turn, will allow smooth execution of the interactive tasks, and you can type other commands in the terminal window while the background job is running. 

By default, all jobs are executed in the foreground. 
You can put a job in the background by suffixing & to the command, for example: updatedb &.
You can use CTRL-Z to suspend a foreground job (i.e., put it in background) and CTRL-C to terminate it. 

You can always use the bg command to run a suspended process in the background, or the fg command to run a background process in the foreground.

---------------------------------------------------------
# Managing Jobs

The jobs utility displays all jobs running in background. The display shows the job ID, state, and command name, as shown here.
jobs -l provides the same information as jobs, and adds the PID of the background jobs.
The background jobs are connected to the terminal window, so, if you log off, the jobs utility will not show the ones started from that window.
---------------------------------------------------------
# The ps Command (System V Style)
ps (process status) provides information about currently running processes keyed by PID. 
If you want a periodic update of this status, you can use top or other commonly installed variants (such as htop, atop, or btop) from the command line, or invoke your distribution's graphical system monitor application (such as gnome-system-monitor or ksysguard).

ps has many options for specifying exactly which tasks to examine, what information to display about them, and precisely what output format should be used.

Without options, ps will display all processes running under the current shell. 

You can use the -u option to display information of processes for a specified username. 

The command ps -ef displays all the processes in the system in full detail. 
The command ps -eLf goes one step further and displays one line of information for every thread (remember, a process can contain multiple threads).

# The Process Tree
pstree displays the processes running on the system in the form of a tree diagram showing the relationship between a process and its parent process and any other processes that it created. 
Repeated entries of a process are not displayed,
 and threads are displayed in curly braces
top

While a static view of what the system is doing is useful, monitoring the system performance live over time is also valuable. 
One option would be to run ps at regular intervals, say, every few seconds. 

A better alternative is to use top to get constant real-time updates (every two seconds by default), until you exit by typing q.top clearly highlights which processes are consuming the most CPU cycles and memory (using appropriate commands from within top).

## First Line of the top Output
The first line of the top output displays a quick summary of what is happening in the system, including:
How long the system has been up
How many users are logged on
What is the load average

The load average determines how busy the system is. A load average of 1.00 per CPU indicates a fully subscribed, but not overloaded, system. 
If the load average goes above this value, it indicates that processes are competing for CPU time. 

If the load average is very high, it might indicate that the system is having a problem, such as a runaway process (a process in a non-responding state).

Second Line of the top Output
The second line of the top output displays the total number of processes, the number of running, sleeping, stopped, and zombie processes. 
Comparing the number of running processes with the load average helps determine if the system has reached its capacity or perhaps a particular user is running too many processes. 
The stopped processes should be examined to see if everything is running correctly.

## Third Line of the top Output
The third line of the top output indicates how the CPU time is being divided between the users (us) and the kernel (sy) by displaying the percentage of CPU time used for each.

The percentage of user jobs running at a lower priority (niceness - ni) is then listed. 
Idle mode (id) should be low if the load average is high, and vice versa. 

The percentage of jobs waiting (wa) for I/O is listed. Interrupts include the percentage of hardware (hi) vs. software interrupts (si). Steal time (st) is generally used with virtual machines, which has some of its idle CPU time taken for other uses.

## Fourth and Fifth Lines of the top Output
The fourth and fifth lines of the top output indicate memory usage, which is divided in two categories:
Physical memory (RAM) – displayed on line 4.
Swap space – displayed on line 5.

Both categories display total memory, used memory, and free space.
You need to monitor memory usage very carefully to ensure good system performance.

 Once the physical memory is exhausted, the system starts using swap space (temporary storage space on the hard drive) as an extended memory pool, and since accessing disk is much slower than accessing memory, this will negatively affect system performance.

If the system starts using swap often, you can add more swap space. However, adding more physical memory should also be considered.

## Process List of the top Output
Each line in the process list of the top output displays information about a process. By default, processes are ordered by highest CPU usage. The following information about each process is displayed:

Process Identification Number (PID)
Process owner (USER)
Priority (PR) and nice values (NI)
Virtual (VIRT), physical (RES), and shared memory (SHR)
Status (S)
Percentage of CPU (%CPU) and memory (%MEM) used
Execution time (TIME+)
Command (COMMAND).

## Interactive Keys with top
Besides reporting information, top can be utilized interactively for monitoring and controlling processes. 
While top is running in a terminal window, you can enter single-letter commands to change its behavior. 
For example, you can view the top-ranked processes based on CPU or memory usage. 

If needed, you can alter the priorities of running processes or you can stop/kill a process.
Commands 	Output
h or ?		Display available interactive keys and their function
t			Display or hide summary information (rows 2 and 3)
m			Display or hide memory information (rows 4 and 5)
l			Show information for each CPU and not just totals
d			Change display update interval
A			Sort the process list by top resource consumers
r			Renice (change the priority of) a specific processes
k			Kill a specific process
f			Enter the top configuration screen
o			Interactively select a new sort order in the process list
Most of these interactive keys are actually toggles; hitting them a second time reverts to the original display. 
There are many more interactive options; see the man page for top for a comprehensive list.

There are a number of alternatives to top with both prettier displays and additional capabilities, including atop, btop and htop; each program has its fans.

We show a screenshot showing all four programs operating simultaneously to get a sense of what they provide.				

---------------------------------------------------------

Scheduling Future Processes Using "at" command
Suppose you need to perform a task on a specific day sometime in the future. 

However, you know you will be away from the machine on that day. How will you perform the task? You can use the at utility program to execute any non-interactive command at a specified time
example 
at now + 2 days
checking on scheduled jobs 
atq
remove the scheduled job
atrm 103
---------------------------------------------------------
# cron
cron is a time-based scheduling utility program. It can launch routine background jobs at specific times and/or days on an ongoing basis. 
cron is driven by a configuration file called /etc/crontab (cron table), which contains the various shell commands that need to be run at the properly scheduled times. 

There are both system-wide crontab files and individual user-based ones. Each line of a crontab file represents a job, and is composed of a so-called CRON expression, followed by a shell command to execute.

Typing crontab -e will open the crontab editor to edit existing jobs or to create new jobs. 
Each line of the crontab file will contain 6 fields
Field		Description	Values
MIN		Minutes		0 to 59
HOUR	Hour field		0 to 23
DOM		Day of Month	1-31
MON		Month field	1-12
DOW		Day of Week	0-6 (0 = Sunday)
CMD		Command	Any command to be executed
cron
cron is a time-based scheduling utility program. It can launch routine background jobs at specific times and/or days on an ongoing basis. 

cron is driven by a configuration file called /etc/crontab (cron table), which contains the various shell commands that need to be run at the properly scheduled times. 

There are both system-wide crontab files and individual user-based ones. Each line of a crontab file represents a job, and is composed of a so-called CRON expression, followed by a shell command to execute.

Typing crontab -e will open the crontab editor to edit existing jobs or to create new jobs. 
Each line of the crontab file will contain 6 fields: 

Table: Fields, Descriptions, Values
Field	Description	Values
MIN	Minutes	0 to 59
HOUR	Hour field	0 to 23
DOM	Day of Month	1-31
MON	Month field	1-12
DOW	Day of Week	0-6 (0 = Sunday)
CMD	Command	Any command to be executed

Examples:
The entry * * * * * /usr/local/bin/execute/this/script.sh will schedule a job to execute script.sh every minute of every hour of every day of the month, and every month and every day in the week.

The entry 30 08 10 06 * /home/sysadmin/full-backup will schedule a full-backup at 8.30 a.m., 10-June, irrespective of the day of the week.

## anacron
While cron has been used in UNIX-like operating systems for decades, modern Linux distributions have moved over to a newer facility: anacron. This was because cron implicitly assumed the machine was always running. However, If the machine was powered off, scheduled jobs would not run. anacron will run the necessary jobs in a controlled and staggered manner when the system is up and running.

The key configuration file is /etc/anacrontab:
anacron
Note that anacron still makes use of the cron infrastructure for submitting jobs on a daily, weekly, and monthly basis, but it defers running them until opportune times when the system is actually alive.
---------------------------------------------------------
# sleep
Sometimes, a command or job must be delayed or suspended. Suppose, for example, an application has read and processed the contents of a data file and then needs to save a report on a backup system. 

If the backup system is currently busy or not available, the application can be made to sleep (wait) until it can complete its work. 
Such a delay might be to mount the backup device and prepare it for writing.  

An even simpler and frequent case is one where a system process needs to run periodically to take care of any work that has been queued up for it to deal with and then has to lurk in the background until it is needed again.

sleep suspends execution for at least the specified period of time, which can be given as the number of seconds (the default), minutes, hours, or days.

After that time has passed (or an interrupting signal has been received), execution will resume.

The syntax is:

sleep NUMBER[SUFFIX]...

where SUFFIX may be:

s for seconds (the default)
m for minutes
h for hours
d for days.
sleep and at are quite different; sleep delays execution for a specific period, while at starts execution at a specific designated later time.
---------------------------------------------------------
# Introduction to Filesystems
“Everything is a file” is an often repeated adage quoted by users of Linux (and all UNIX-like operating systems). 

Whether you are dealing with normal data files and documents, or with devices such as sound cards and printers, this means interaction with them proceeds through the same Input/Output (I/O) operations you commonly use with files. 

On many systems (including Linux), the filesystem is structured like a tree. 
The tree is usually portrayed as inverted and starts at what is most often called the root directory, which marks the beginning of the hierarchical filesystem and is also sometimes referred to as the trunk and simply denoted by /. 

The root directory is not the same as the root user. The hierarchical filesystem also contains other elements in the path (directory names), which are separated by forward slashes (/), as in /usr/bin/emacs, where the last element is the actual file name.

## Filesystem Varieties
Linux supports a number of native filesystem types, expressly created by Linux developers, such as:

ext3
ext4
squashfs
btrfs
It also offers implementations of filesystems used on other alien operating systems, such as those from:
Windows (ntfs, vfat, exfat)
SGI (xfs)
IBM (jfs)
MacOS (hfs, hfs+)
Many older, legacy filesystems, such as FAT, are also supported.  The most advanced filesystem types in common use are the journaling varieties: ext4, xfs, btrfs, and jfs. 

These have many state-of-the-art features and high performance, and are not easy to corrupt accidentally.

Linux also makes use of network (or distributed)  filesystems, where all or part of the filesystem is on external machines. 

Besides NFS (Network File System) whose usage we will discuss, this includes Ceph, Lustre, and OpenAFS.

## Linux Partitions
In most situations, each filesystem on a Linux system occupies a disk partition.   
Partitions help to organize the contents of disks according to the kind and use of the data contained. 

For example, important programs required to run the system are often kept on a separate partition (known as root or /) than the one that contains files owned by regular users of that system (/home). 

In addition, temporary files created and destroyed during the normal operation of Linux may be located on dedicated partitions. One advantage of this kind of isolation by type and variability is that when all available space on a particular partition is exhausted, the system may still operate normally.

Furthermore, if data is either corrupted through error or hardware failure, or breached through a security problem,  it might be possible to confine problems to an area smaller than the entire system. 

## Mount Points (/)
Before you can start using a filesystem, you need to mount it on the filesystem tree at a mount point. 

This is simply a directory (which may or may not be empty) where the filesystem is to be grafted on. 

Sometimes, you may need to create the directory if it does not already exist.
WARNING: If you mount a filesystem on a non-empty directory, the former contents of that directory are covered-up and not accessible until the filesystem is unmounted. 
Thus, mount points are usually empty directories.
 Mounts Points /  - /dev/sda1 - /dev/sda
 Mounts Points /  - /home - /dev/sda5 - /dev/sda
Mounts points / - /var - /dev/sda6 - /dev/sda

## Mounting and Unmounting
The mount command is used to attach a filesystem (which can be local to the computer or on a network) somewhere within the filesystem tree. 
The basic arguments are the device node and mount point. For example,

$ sudo mount /dev/sda5 /home

will attach the filesystem contained in the disk partition associated with the /dev/sda5 device node into the filesystem tree at the /home mount point. 
There are other ways to specify the partition other than the device node, such as using the disk label or UUID (Universally Unique IDentifier).
To unmount the partition, the command would be:

$ sudo umount /home

Note the command is umount, not unmount! Only a root user (logged in as root, or using sudo) has the privilege to run these commands, unless the system has been otherwise configured.

If you want it to be automatically available every time the system starts up, you need to edit /etc/fstab accordingly (the name is short for filesystem table). 

Looking at this file will show you the configuration of all pre-configured filesystems. man fstab will display how this file is used and how to configure it.

Filesystem Table
Executing mount without any arguments will show all presently mounted filesystems. 
The command df -Th (disk free) will display information about mounted filesystems, including the filesystem type, and usage statistics about currently used and available space.

## Mounting and Unmounting
You may notice a number of entries of type tmpfs. These are not real physical filesystems but are parts of system memory that are represented as such to take advantage of certain programming features.
---------------------------------------------------------
# The /dev Directory
The /dev directory contains device nodes, a type of pseudo-file used by most hardware and software devices, except for network devices. This directory is:

Empty on the disk partition when it is not mounted
Contains entries which are created by the udev system, which creates and manages device nodes on Linux, creating them dynamically when devices are found. 
The /dev directory contains items such as:
/dev/sda1 (first partition on the first hard disk)
/dev/lp1 (second printer)
/dev/random (a source of random numbers).

---------------------------------------------------------
## The /var Directory
The /var directory contains files that are expected to change in size and content as the system is running (var stands for variable), such as the entries in the following directories:

System log files: /var/log
Packages and database files: /var/lib
Print queues: /var/spool
Temporary files: /var/tmp.

## The /var Directory
The /var directory may be put on its own filesystem so that growth of the files can be accommodated and any exploding file sizes do not fatally affect the system. 

Network services directories such as /var/ftp (the FTP service) and /var/www (the HTTP web service) are also found under /var.
---------------------------------------------------------
## The /boot Directory
The /boot directory contains the few essential files needed to boot the system. For every alternative kernel installed on the system there are four files:

vmlinuz
The compressed Linux kernel, required for booting.
initramfs
The initial ram filesystem, required for booting, sometimes called initrd, not initramfs.

config
The kernel configuration file, only used for debugging and bookkeeping.
System.map
Kernel symbol table, only used for debugging.
Each of these files has a kernel version appended to its name.

The Grand Unified Bootloader (GRUB) files such as /boot/grub/grub.conf or /boot/grub2/grub2.cfg are also found under the /boot directory.

The /boot Directory

The screenshot shows an example listing of the /boot directory, taken from a RHEL system that has multiple installed kernels, including both distribution-supplied and custom-compiled ones. 

Names will vary and things will tend to look somewhat different on a different distribution.
---------------------------------------------------------
The /lib and /lib64 Directories
/lib contains libraries (common code shared by applications and needed for them to run) for the essential programs in /bin and /sbin. These library filenames either start with ld or lib. For example, /lib/libncurses.so.5.9.

Most of these are what is known as dynamically loaded libraries (also known as shared libraries or Shared Objects (SO)). 
On some Linux distributions there exists a /lib64 directory containing 64-bit libraries, while /lib contains 32-bit versions.
---------------------------------------------------------
# Using the file Utility
In Linux, a file's extension does not, by default, categorize its nature the way it might in other operating systems. 
For example, one cannot assume that a file named file.txt is a text file and not an executable program. 

In Linux, a filename is generally more meaningful to the user of the system than the system itself. 

In fact, most applications directly examine a file's contents to see what kind of object it is rather than relying on an extension. 

This is very different from the way Windows handles filenames, where a filename ending with .exe, for example, represents an executable binary file.

The real nature of a file can be ascertained by using the file utility. For the file names given as arguments, it examines the contents and certain characteristics to determine whether the files are plain text, shared libraries, executable programs, scripts, or something else.
---------------------------------------------------------
# Backing Up Data
There are many ways you can back up data or even your entire system. Basic ways to do so include the use of simple copying with cp and use of the more robust rsync.

Both can be used to synchronize entire directory trees. However, rsync is more efficient, because it checks if the file being copied already exists. 
If the file exists and there is no change in size or modification time, rsync will avoid an unnecessary copy and save time. Furthermore, because rsync copies only the parts of files that have actually changed, it can be very fast.

cp can only copy files to and from destinations on the local machine (unless you are copying to or from a filesystem mounted using NFS), but rsync can also be used to copy files from one machine to another. 

Locations are designated in the target:path form, where target can be in the form of someone@host. The someone@ part is optional and used if the remote user is different from the local user.

rsync is very efficient when recursively copying one directory tree to another, because only the differences are transmitted over the network.

One often synchronizes the destination directory tree with the origin, using the -r option to recursively walk down the directory tree copying all files and directories below the one listed as the source.
---------------------------------------------------------
# Using rsync
rsync is a very powerful utility. For example, a very useful way to back up a project directory might be to use the following command:

$ rsync -r project-X archive-machine:archives/project-X

Note that rsync can be very destructive! Accidental misuse can do a lot of harm to data and programs, by inadvertently copying changes to where they are not wanted. 

Take care to specify the correct options and paths. It is highly recommended that you first test your rsync command using the -dry-run option to ensure that it provides the results that you want.

To use rsync at the command prompt, type rsync sourcefile destinationfile, where either file can be on the local machine or on a networked machine; The contents of sourcefile will be copied to destinationfile.

A good combination of options is shown in:

$ rsync --progress -avrxH  --delete sourcedir destdir
---------------------------------------------------------
# Compressing Data
File data is often compressed to save disk space and reduce the time it takes to transmit files over networks.

Linux uses a number of methods to perform this compression, including:
Table: Methods to Perform Compression
Command	Usage
gzip			The most frequently used Linux compression utility
bzip2		Produces files significantly smaller than those produced by gzip
xz			The most space-efficient compression utility used in Linux
zip			Is often required to examine and decompress archives from other operating systems

These techniques vary in the efficiency of the compression (how much space is saved) and in how long they take to compress; generally, the more efficient techniques take longer.
Decompression time does not vary as much across different methods.

In addition, the tar utility is often used to group files in an archive and then compress the whole archive at once.

Compress Data Using gzip
gzip has historically been the most widely used Linux compression utility. It compresses well and is very fast.
The following table provides some usage examples:
Table: gzip Usage Examples

# `gzip` Commands

| Command             | Usage                                                                                                                                          |
|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| `gzip *`            | Compresses all files in the current directory; each file is compressed and renamed with a `.gz` extension.                                     |
| `gzip -r projectX`  | Compresses all files in the `projectX` directory, along with all files in all of the directories under `projectX`.                            |
| `gunzip foo`        | De-compresses `foo` found in the file `foo.gz`. Under the hood, `gunzip` is actually the same as `gzip -d`.                                   |
---------------------------------------------------------
# Compressing Data Using bzip2

bzip2 has a syntax that is similar to gzip but it uses a different compression algorithm and produces significantly smaller files, at the price of taking a longer time to do its work. 
Thus, it is more likely to be used to compress larger files.
# `bzip2` Commands

| Command          | Usage                                                                                                                                                      |
|------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `bzip2 *`        | Compresses all files in the current directory and replaces each file with a file renamed with a `.bz2` extension.                                         |
| `bunzip2 *.bz2`  | Decompresses all files with an extension of `.bz2` in the current directory. Under the hood, `bunzip2` is the same as calling `bzip2 -d`.                 |

NOTE: bzip2 has lately become deprecated due to lack of maintenance and the superior compression ratios of xz which is actively maintained. 

While it should no longer be used for compressing files, you are likely to still need it to decompress files you encounter with the bz2 extension.
---------------------------------------------------------
# Linux Directory 
/ The root directory, where everything begins. 

/bin Contains binaries (programs) that must be present for the system to boot and run. 

/boot Contains the Linux kernel, initial RAM disk image (for drivers needed at boot time), and the boot loader. 
Interesting files include /boot/grub/grub.conf, or menu.lst, which is used to configure the boot loader, and /boot/vmlinuz (or something similar), the Linux kernel.

/dev This is a special directory that contains device nodes. “Everything is a file” also applies to devices. 
Here is where the kernel maintains a list of all the devices it understands

/etc The /etc directory contains all the system-wide configuration files. It also contains a collection of shell scripts that start each of the system services at boot time. 
Everything in this directory should be readable text. While everything in /etc is interesting, here are some all-time favorites: /etc/crontab, a file that defines when automated jobs will run; /etc/fstab, a table of storage devices and their associated mount points; and /etc/passwd, a list of the user accounts.

/home In normal configurations, each user is given a directory in /home. Ordinary users can write files only in their home directories. 
This limitation protects the system from errant user activity.

/lib Contains shared library files used by the core system programs. These are similar to dynamic link libraries (DLLs) in Windows.

/lost+found Each formatted partition or device using a Linux file system, such as ext3, will have this directory. 
It is used in the case of a partial recovery from a file system corruption event. 
Unless something really bad has happened to your system, this directory will remain empty.

/media On modern Linux systems, the /media directory will contain the mount points for removable media such as USB drives, CD-ROMs, and so on, that are mounted automatically at insertion.

/mnt On older Linux systems, the /mnt directory contains mount points for removable devices that have been mounted manually.

/opt The /opt directory is used to install “optional” software. This is mainly used to hold commercial software products that might be installed on the system.

/proc The /proc directory is special. It’s not a real file system in the sense of files stored on your hard drive. Rather, it is a virtual file system maintained by the Linux kernel. The “files” it contains are peepholes into the kernel itself. The files are readable and will give you a picture of how the kernel sees your computer.

/root This is the home directory for the root account.

/sbin This directory contains “system” binaries. 
These are programs that perform vital system tasks that are generally reserved for the superuser.

/tmp The /tmp directory is intended for the storage of temporary, transient files created by various programs. 
Some configurations cause this directory to be emptied each time the system is rebooted.

/usr The /usr directory tree is likely the largest one on a Linux system. 
It contains all the programs and support files used by regular users.

/usr/bin /usr/bin contains the executable programs installed by your Linux distribution. 
It is not uncommon for this directory to hold thousands of programs.

/usr/lib The shared libraries for the programs in /usr/bin.

/usr/local The /usr/local tree is where programs that are not included with your distribution but are intended for system-wide use are installed. 
Programs compiled from source code are normally installed in /usr/local/bin. 
On a newly installed Linux system, this tree exists, but it will be empty until the system administrator puts something in it.

/usr/sbin Contains more system administration programs.

/usr/share /usr/share contains all the shared data used by programs in /usr/bin. 
This includes things such as default configuration files, icons, screen backgrounds, sound files, and so on.

/usr/share/docMost packages installed on the system will include some kind of documentation. 
In /usr/share/doc, we will find documentation files organized by package.

/var With the exception of /tmp and /home, the directories we have looked at so far remain relatively static; that is, their contents don’t change. 
The /var directory tree is where data that is likely to change is stored. Various databases, spool files, user mail, and so forth, are located here.

/var/log /var/log contains log files, records of various system activity. 
These are important and should be monitored from time to time. 
The most useful ones are /var/log/messages and /var/log/syslog. 
Note that for security reasons on some systems, you must be the superuser to view log files.

----------------------------
# Web Server and Firewall stuffs
```bash
sudo dnf/apt install nginx
systemctl start nginx
systemctl enable nginx
systemctl status nginx
```
notes: enable command is for boot. 
curl command is like a browser but in the terminal. it fetches a webpage and dumps the HTML to the screen.
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
<!--  -->

















































































































