## Overview of Linux Architecture
- Linux filesystem comprises of 5 layers
	- User
	- Application
		- System Daemons
		- Shells
		- User Apps
		- Tools
	- Operating System
		- job scheduling
		- Keeping track of time
		- performs file management
	- Kernel- core component that starts on boot and remain in memory
		- bridge between apps and hardware
		- managing memory
		- processing
		- security
	- Hardware
		- CPU
		- RAM
		- Storage
		- Screen
		- USB devices

### Linux Filesystem
![[Pasted image 20220908122600.png]]


- How are commands run?
	- So here the user runs a command in his/her terminal which is then relayed to the Shell
	- The core components of the OS and the kernel translate the command for the hardware to perform
	- When the hardware completes the command, the kernel reads any changes or results and sends them back via shell to the terminal for the user's information

- ~ home directory
- / root directory
- .. parent directory
- . current directory

### Text editors

- Command-line text editors
	- GNU nano
	- vi
	- vim
- GUI-based editor
	- gedit
- Command-line or GUI
	- emacs

- To open nano 
	> nano #filename 

#### Vim

- To open vim 
	> vim 
	
- Insert mode
	- press i to enter insert mode
	- type some text
	- press esc to exit and enter command mode
- Command mode
	> :sav #filename 
		- this command creates a file and writes the buffer to the file

	> :w (to write buffer to file without exiting)
	> :q (to quit the vim session)
	> :q! (to quit without saving)
	

### Packages and Package managers

- Packages: software updates and software installations files for linux OS are distributed in files known as packages
- Package managers are used to manage the download and installation of package managers

- Deb and RPM packages are used by package managers in Linux OS
- .deb is used for debian based distros
- .rpm files are used for Red Hat based distros such as CentOS/ RHEL, Fedora, openSUSE
- RPM stands for Red Hat package manager

- To convert packages from RPM format to deb we use the alien command
	> alien #package-name.rpm

- To convert from deb to RPM
	> alien -r #package-name .deb

- To update deb based linux systems we use apt
- To update RPM based linux systems we use PackageKit which is a GUI tool
- To update RPM based linux systems we use yum, that stands for YellowDog Updater, Modified.
	> sudo yum install #package-name 
	


## Overview of Common Linux Shell Commands
- Linux Shell enables access to files, utilities, and applications, is also an interactive scripting language that can be used to automate tasks.

> printenv shell (returns path to the default shell program)
> bash (to switch to bash)


- whoami: username
- id: user ID or group ID
- uname: operating system name
- ps: running processes
- top: resource usage
- df: mounted file systems
- man: reference manual
- date: today's date

- more: print file contents page-by-page
- head: print first N lines of file
- tail: print last N lines of file

- tar: archive set of files
- zip: compress set of files

- hostname: prints hostname
- wget: download file from url

> uname -s -r (returns OS name and version)
> uname -v (returns all version information)


> df -h ~ (returns mounted disk information specific to home directory with usage percentage)
> df -h (returns all the mounted disks on your system)

> top -n 3 (lists the top 3 processes or task running on system)

> find (used to find files in directory tree)
> find . -name " #filename" (dot here instructs the command to find in the current working directory)
> find . -iname " #filename " (performs insensitive verison of search we use -iname)


> rmdir (solely used for empty directories)
> rm -r (removes directory along with all its child file objects)


> date -r #filename  (returns date of last modified or created date)


> cp -r #directory-name (to copy directory and recursively copy all subdirectories)


> mv #directory-to-move-from #directory-to-move-to


- To archive files using tar
	> tar -cf #archiveName.tar #directory-to-archive
	
	- the c option states to create a new archive and the f option states tar to interpret its input from file
- to compress the archived file 
	> tar -czf #compressedArchiveName.tar.gz #directory-to-archive 
	> tar -tf #archivedDirectory (returns contents of archived directory)

- to extract archived files using tar
	> tar -xf #archiveName #directory-to-extract-to

- to decompress the archived file
	> tar -xzf #compressedArchiveName #directory-to-decompress-to 
	


- Zip command can compress files and directories to an archives
	- The main difference is that first the compression happens first and then it is archived whereas in tar it is archived first and then compressed
	> zip #zippedDirectoryName #directory-to-zip
	

- Unzip extracts and decompresses archive
	> unzip #zippedDirectoryName 
	> ls -R (to list files and folders)
	

### Networking Commands
> hostname -i (provides IP address of hostname)
> 


## Shell Scripting Basics

- Scripts are generally used to automate processes such as ETL jobs, file backups and archiving and general admin tasks
- shell script directives (to let the interpreter know thast it is a bash script)
	> #!/bin/bash
	
- python script directives
	> #!/usr/bin/env python3
	

- pipe command |
	> command 1 | command 2
	
	- output of command 1 is the input of command 2
	- piep stands for pipeline

- Environment variables are shell varibales with scope extended to all child processes of the shell.
- They are created using the export command and listed using the env command

- Quoting and metacharacters
	- \\ is an escape special character interpretation
	- " " interpret literally, but evaluate metacharacters
	- ' ' interpret everything literally
	- * is a filename expansion wildcard
	- ? is a single character wildcard in filename expansion


- I/O Redirection
	- > redirects output to a file and also creates file and overwrites file if content already exists
	- >> appends output to file
	- 2> redirects standard error to a file
	- 2>> appends standard error to a file
	- < used to pass file contents as input

- Batch mode vs. Concurrent mode
	- Batch mode: runs commands seqeuntially that is one after the other
	- Concurrent mode: runs commands in parallel to each other
		> command1 & command2 (here command1 runs in the background waiting for command2 to run in the foreground)\
		
- Q) _Write a shell script named `latest_warnings.sh` that prints the latest 5 warnings from the /var/log/bootstrap.log file.
	> #!/bin/bash
	> grep warning /var/log/boostrap.log | tail -5
	
### Scheduling cron jobs with crontab
- use [[Bash Scripting]] for refrence on cron  jobs


### Shell Script: Conditionals
- Every `if` condition block must be paired with a `fi`.
- You must always put spaces around your conditions in the `[` `]`.
- Alternatively you can use test instead of []

- Nested ifs example
	> a=3 
	> b=3 
	> c=3
	> if test $a == $b
	> then
	> 	if test $a == $ c
	> 	then
	> 		if test $b == $c 
	> 		then 
	> 			echo "all the variables a b and c are equal"
	> 		fi
	> 	fi
	> else
	> 	echo "not equal"
	> fi

- optimized code of above logic
	> a=3
	> b=3
	> c=3
	> if test $a == $b && test $a == $c && test $b == $c
	> then 
	> 	echo "all a b c are equal"
	> else
	> 	echo "a b and c aren't equal"
	> fi




# Final Project

## Scenario

You are a lead linux developer at the top-tech company "ABC International INC." ABC currently suffers from a huge bottleneck - each day, interns must painstakingly access encrypted password files on core servers, and backup those that were updated within the last 24-hours. This introduces human error, lowers security, and takes an unreasonable amount of work.

As ABC INC's most trusted linux developer, you have been tasked with creating a script `backup.sh` which automatically backs up any of these files that have been updated within the past 24 hours.

