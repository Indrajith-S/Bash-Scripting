- BASH - Bourne again shell
- To check the shell we're in
	> echo $SHELL
	
- To list all files under a directory (even hidden ones)
	> ls -al

	"a" stands for all and "i" stands for long list
	In the output columns 
		1) dash denotes files
		2) d denotes directory
		3) the r, w and x denote read write and execute perms
		4) rhyme are users and groups
		5) the next column is the size in bytes
		6) file name is in the last column

- To rename a file 
	> mv #oldFileName #newFileName

- rm to delete a file
- To copy content from one file to another file
> cat #oneFile > #anotherFile
> "less" #filename  to scroll through file contents

- nano is a text editopr used in the shell itself
	> nano #filename 
	CTRL + S to save file
	CTRL + X to exit

- "mkdir" to make a new directory 
- "touch" to add new files into the directory
- "rmdir" to delete only EMPTY directories
- "rm -ir #dirname
	- "i" confirms if you want to delete everything in the directory
	- "r" recursively deletes everything in the directory and the then the directory itself

> find / -name "backup" 2>/dev/null

	- This command here is finding a file with name backup in it starting the search from the directory itself and 2> is pushing all the errors if any to /dev/null

> history

	- this command gives you the history of all the commands we used

### Aliases

- To keep aliases in a file
	> nano .bash aliases
	> alias fbackup= 'find ~ -name "backup*" 2>/dev/null'
		- so here we're creationg an alias named fbackups and to that we're assigning the above command
		- Now to get the bash shell to look at the file immediately
			> source .bash_aliases
			
	> backup
		- will give you the files with name backup in it


### Writing a Shell Script

> nano myBackup
> #!/bin/bash
	- this is a note to the interpreter that this is a bash file
- Anything starting with a # is a comment
> # myBackup: backup utility for dev directory
- creating variable first
	> BACKUP_PATH= "/home/rhyme/dev/"
	> HOME_PATH= "/home/rhyme/"
	> DATE= `date +%d%m%Y`
		- backtics tell the shell to execute whats inside
		- DATE here is a variable that holds the output from calling the date command
	> BACKUP "backup_"
	> EXT+".tar"
		- this tell the script to look for files with the name backup and extension tar
	> FILE_NAME=$HOME_PATH $BACKUP $DATE $EXT
		- here we are referencing the variable using "$" sign
	> echo $FILE_NAME
	> ./myBackup
	
	> tar cfz $FILE_NAME $BACKUP_PATH
		- this archives the dev directory and saves it as this filename
		- tar stands for tape archive
		- this tells tar to create a file with our filename and use the backup path ie our dev directory here
		- "z" here zips the script for us

- To summarise
	> #!/bin/bash
	> #myBackup: backup utility for dev directory
	> BACKUP_PATH= "/home/rhyme/dev/"
	> HOME_PATH= "/home/rhyme/"]
	> DATE= `date +%d% m%Y`
	> BACKUP "backup_"
	> EXT+".tar"
	> FILE_NAME=$HOME_PATH $BACKUP $DATE $EXT
	> #echo $FILE_NAME
	> tar cf $FILE_NAME $BACKUP_PATH

- now we have a bash script that will create a daily backup file 


- Permisions
	> chmod u=rwx myBackup
		- what this does is it gives the user read write and execute perms on the myBackup script
	> chmod go=rx myBackup
		- gives groups and others only read and execute perms of myBackup script
	> ll myBackup
		- to view the perms
	> chmod o-r myBackup
		- to remove read perms from others
		- "+r" to add those perms back

## Bash Scripting and Creating a Cron Job with Crontab

> if test -f "$FILE_NAME"; then
       > mail -A $FILE_NAME -s "TODAY's BAckup" johndoe@gmail.com 
       
       - this will mail my backup in case the backup fails
> else 
       > echo $DATE " There was a problem creating ther backup file" >> $HOME_PATH/error.log
> fi

- if the file doesn't exist then it will send out the message and putting it in an error log file by creating it

- crontab A.K.A cron table is a job scheduler on Unix-like operating systems.
- one uses cron to schedule jobs, also known as cron jobs, to run periodically at fixed times, dates, or intervals.
- To create a new crontab
	> crontab -e
	- m is for minute (1-60)
	- h is for hour (1-12)
	- dom is day of the month (1-31)
	- mon for month (1-12)
	- dow is day of the week (1-7)

> 0 2 * * * /home/rhyme/myBackup
	- This command makes our crontab for making daily backups at 2am everyday
	- * stands for all
	
- to run a script every minute
	> * * * * * /path/to/script
	
- to delete all crontabs on my user account
	> crontab -r
	
- shortcuts to run crontab
	> @yearly /path/to/job
	> @annually /path/to/job
		- These are the same. It will run once a year.
	> @monthly /path/to/job 
		- runs once per month.
	> @weekly /path/to/job
		-Run once a week.
	> @daily /path/to/job
	> @midnight /path/to/job
		- And these are the same-- you'll run once a day.
		- Daily is specific--it would schedule to run at midnight. This would be the same as '0 0 * * *'.

	> @hourly /path/to/job
		- runs once every hour.
	> @reboot /path/to/job
		- runs every time the system boots up
		 
