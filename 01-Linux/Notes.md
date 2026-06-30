**Intro**
Using the terminal
CTRL + ALT + T (Shortcut to open Terminal)
'ls' - to see current directory contents
'pwd' print working directory - Shows current directory
'cd desktop' - change directory to desktop
'mkdir hello' - makes new directory hello
'rmdir hello' - deletes hello directory
'touch hello.txt' - creates file
'rm hello.txt' - deletes file
Using tab to autocomplete

**What are Commands?**
Linux commands are textual instructions that tell the OS what to do
Commands are case sensitive
Have various options and arguments to modify behaviour
Example. 'ls' -command '-a' - options '.' - arguments
All commands have instructions manuals
'man ls' - Shows manuals for ls command

**Creating Files and echo, grep, cat**
'echo "hello" > file.txt' - writes hello to file.txt
Above command overwrite existing text with hello
cat file.txt - to see content of file.txt
'grep' - used to search for file and specific patterns 
'grep "hello" file.txt' - search for the word hello in file.txt
'echo "Test"' >> file.txt - Apends Test to file.txt

**Other Shells**
Most Linux distros come with bash shell. Which is the widest used
Some others are. Zsh. Ksh. Fish. etc
Zsh. Is preferred for Developers.
'sudo apt-get install zsh' - Install zsh shell
'sudo chsh -s /bin/zsh' - change default shell to zsh
Restart shell required
'sudo chsh -s $(which zsh) $(whoami)' - run this if not worked

**Linux File Systems**
Hierarchical root structure
![[LinuxFileSystem.png]]

**Creating file. touch and echo**
touch is a simple tool. Can be used to create files but also update timestamps
'touch myfile.txt' - creates myfile.txt
Running this again updates timestamp without changing file contents
'ls -l' - to see file permissions & timestamp
'echo' is used to repeat text but can also be used to write files to txt
'echo "Hello World" > myfile1.txt' - will overwrite all existing txt with Hello World
'echo "Hello World" >> myfile1.txt' - will append txt. Not overwrite

**Concatenate command 'cat'**
This command is used to see contents of a file
Can also be used to combine files
'cat myfile1.txt myfile2.txt > combined.txt' merges content of both files and outputs to combined.txt
'cat myfile2.txt >> myfile1.txt' - merges contents from file2 to file1

**'head' and 'tail' commands**
Used for viewing the beginning and end of files
'head' by default shows first 10 lines of txt
'tail' by default shows last 10 lines of txt
'head -n 5' will show first 5 lines
'head -n 10 multi.txt | tail -n 5' - gets first 10 lines. Then output last 5 of those.

**File Management 'cp' 'mv' 'rm' commands**
'cp' - copy - 'mv' - move - 'rm' - remove/delete
'cp multi.txt' 'multi2.txt' - makes a copy of multi.txt as multi2.txt
'cp -r directory directory2' - to copy directories -r is required
'mv' - is used to move or rename files
'mv multi2.txt newmulti.txt' - this will rename mutlt2.txt to newmulti.txt
'mv newmulti.txt directory2' - moves newmulti to directory2
'rm' - is used to remove/delete files
'rm newmulti.txt' -  will remove newmulti.txt
'rm -r directory2' - will remove directory2

**Directories - 'mkdir' 'rmdir' 'rm -r'**
'mkdir' - make directory
'mkdir projects' - makes projects directory
'mkdir -p projects/src/components' - makes src folder within projects and components within src
'ls -R' - lists directories recursively - show full nested directories
'rmdir' - to remove directories
'rmdir projects' - to remove empty directories
'rm -r projects' -  to remove directory and contents

**Dealing with spaces in Folder Names**
'mkdir "My Folder"' - speech marks are required if file name has spaces
'mkdir My\ Folder\ 2' - or you can use backslash before the space so the terminal ignores the spaces
'cd "My Folder"' - to navigate to My Folder directory
You can also use tab (In zsh terminal) to quickly select

**VIM Text Editor**
Most widely used text editor on Linux
'vim example.txt' - if file does not exist it will create file and enter text VIM
Default open to command mode
VIM has several modes. Few important ones are. Command, Insert, & visual mode.
To switch to insert mode press 'i' - here you will be able to add text
To switch back to command mode press 'esc'
In command mode you can use 'h' 'j' 'k' 'l' to move left down up & right.
To save changes and exit. Make sure in command mode then type ':wq' or ':wq!'
To quit without saving changes 'q' or 'q!'
':w' - to just save

**VIM navigation**
'0' - to move to the beginning of the line
'Shift + 4' - to move to the end of the line
'w' - to move forward by a word
'b' - to move back by a word
':3' - to move to line number 3
'/example' - to find word example
'n' & 'N' to navigate between the found word example
'dd' - deletes whole line in command mode
'D' - deletes from the cursor to the end of the line
'y' - to copy/yank line
'p' - to paste yanked line
'u' - undo last change
'CTRL + R' - to redo change
':syntax on' - in command mode to highlight commands
':set number' - show line numbers
':set nonumber' - to remove line number

**sudo command**
Stands for super user do. Command is used for elevated tasks
'sudo apt-get update' - For example this command can only be run as super
'sudo !!' - to rerun previous command you forgot to use sudo
'sudo su' - this permanently changes default to root (super) user
'whoami'- to confirm you are running as root user
'/var/log/auth.log' - stores all sudo command history

**Users**
'sudo useradd newuser' - create new user
'sudo passwd newuser' - sets password for new user
'su - newuser' - switch to new user
'sudo usermod -aG sudo newuser' - add newuser to be able to use sudo
'sudo deluser newuser sudo' - remove new user from sudo

**Groups**
'cat /etc/group' - contains all group information
'sudo groupadd devops' - create a groups called devops
'sudo usermod -aG devops newuser' - add new user to devops group
Every user is part of a group with same name as their account by default
'sudo gpasswd -d newuser devops' - remove new user from devops group
'sudo groupdel devops' - delete devops group
'grep devops /etc/group' - confirm group no longer exists
'sudo usermod -aG group1,group2 newuser' - can add to multiple groups

**File Permissions**

![[LinuxFilePermissions.png]]
r - read w - write e - execute
'ls -l' - to see file permissions in the current working directory


![[LinuxBinaryOctalString.png]]
**Managing Permissions'**
'chmod u+x,g+r,o-w test.txt' - this will add execute to user, read to group, and remove write from others (public)
https://chmod-calc-five.vercel.app/ - using octal calculator you can also use below
'chmod 750 test.txt'
'chmod +x test.txt' - this will give execute permissions to everyone. Useful for script files
'chmod ug=rw,o=r test.txt' - give user & group read write, and others just read
'chown' - change owner
'chgrp' - change group
'sudo chown newuser test.txt' - change owner of test.txt to newuser
'sudo chgrp newgroup test.txt' - change owner of test.txt to newgroup
'sudo chown newuser:newgroup test.txt' - change both owner and group
'sudo chown -R newuser:newgroup directory1' - change owner/group of directory and it's contents

![[LinuxStandardStreams.png]]

**Environment Variables**
These are variables that are set in the environment and affect the behaviour of processes on your system.
There are present variables, but you can also set your own.
'printenv' - shows current environment variables
'echo $HOME' - to check a single variable such as home
'export MY_VAR="Hello World"' - to create a temp variable
'vim .zshrc' - to edit permanent variables and add new. On the last line you can add same line you used to create temp
'source .zshrc' - to apply new perm variable. Restart of terminal required

**Aliases**
Useful for shortening commands you often use
alias hello='echo "Hello World!"' - to set alias for hello
To make this permanent update .zshrc

**Linux CLI Shortcuts**
'history' - to view a list of commands run
If you want to run a particular command from history again
'!20' - for example run command 20 on the list again
Pressing tab reveals the command to confirm
'CTRL + R' - allows you to do a reverse search
After typing in search, repeating 'CTRL + R' cycles through commands
