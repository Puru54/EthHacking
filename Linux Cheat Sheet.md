
#linux #commands
This cheat sheet covers basic to advanced commands for Linux, including file management, system monitoring, networking, and text editing with `vim` and `nano`.


---

## Basic Linux Commands

|Command|Description|
|---|---|
|`ls`|List files and directories|
|`pwd`|Print working directory|
|`cd <directory>`|Change directory|
|`cd ..`|Move up one directory|
|`cd ~`|Go to home directory|
|`mkdir <dir_name>`|Create a directory|
|`rmdir <dir_name>`|Remove an empty directory|
|`touch <file_name>`|Create an empty file|
|`clear`|Clear the terminal screen|
|`man <command>`|Display manual for a command|
|`echo <text>`|Print text to the terminal|
|`cat <file>`|Display file content|
|`history`|Show command history|
|`exit`|Exit the terminal|

---

## File Management

|Command|Description|
|---|---|
|`cp <source> <destination>`|Copy files or directories|
|`mv <source> <destination>`|Move or rename files/directories|
|`rm <file>`|Remove a file|
|`rm -r <directory>`|Remove a directory recursively|
|`ln -s <target> <link_name>`|Create a symbolic link|
|`find <path> -name <filename>`|Search for files|
|`grep <pattern> <file>`|Search for text in a file|
|`tar -cvf <archive.tar> <files>`|Create a tar archive|
|`tar -xvf <archive.tar>`|Extract a tar archive|
|`zip <archive.zip> <files>`|Create a zip archive|
|`unzip <archive.zip>`|Extract a zip archive|

---

## System Monitoring

|Command|Description|
|---|---|
|`top`|Display real-time system processes|
|`htop`|Interactive process viewer (install with `sudo apt install htop`)|
|`ps`|Display running processes|
|`ps aux`|Display all running processes|
|`free -h`|Show memory usage|
|`df -h`|Display disk space usage|
|`du -sh <directory>`|Show directory size|
|`uptime`|Show system uptime|
|`uname -a`|Display system information|
|`whoami`|Show current user|

---

## Networking

|Command|Description|
|---|---|
|`ping <host>`|Ping a host|
|`ifconfig`|Display network interfaces (deprecated, use `ip`)|
|`ip addr show`|Show IP addresses|
|`netstat`|Display network connections|
|`ssh <user@host>`|Connect to a remote host via SSH|
|`scp <file> <user@host:path>`|Copy files over SSH|
|`wget <url>`|Download files from the web|
|`curl <url>`|Transfer data from/to a server|
|`nslookup <domain>`|Query DNS|
|`dig <domain>`|DNS lookup utility|

---

## Permissions and Ownership

|Command|Description|
|---|---|
|`chmod <permissions> <file>`|Change file permissions|
|`chown <user>:<group> <file>`|Change file owner and group|
|`chgrp <group> <file>`|Change file group|
|`umask`|Set default file permissions|

### Permission Codes

- `4` = Read (r)
    
- `2` = Write (w)
    
- `1` = Execute (x)
    

Example: `chmod 755 <file>` gives `rwxr-xr-x`.

---

## Process Management

|Command|Description|
|---|---|
|`kill <PID>`|Terminate a process by PID|
|`killall <process_name>`|Terminate all processes by name|
|`pkill <process_name>`|Kill processes by name|
|`bg`|Resume a job in the background|
|`fg`|Bring a job to the foreground|
|`jobs`|List background jobs|
|`nohup <command>`|Run a command immune to hangups|

---

## Package Management

### Debian/Ubuntu (APT)

|Command|Description|
|---|---|
|`sudo apt update`|Update package list|
|`sudo apt upgrade`|Upgrade installed packages|
|`sudo apt install <package>`|Install a package|
|`sudo apt remove <package>`|Remove a package|
|`sudo apt autoremove`|Remove unused packages|
|`sudo apt search <keyword>`|Search for a package|

### Red Hat/CentOS (YUM/DNF)

|Command|Description|
|---|---|
|`sudo yum install <package>`|Install a package|
|`sudo yum remove <package>`|Remove a package|
|`sudo yum update`|Update all packages|
|`sudo dnf install <package>`|Install a package (DNF)|

---

## Text Editing with Vim

### Basic Vim Commands

|Command|Description|
|---|---|
|`vim <file>`|Open a file in Vim|
|`i`|Enter insert mode|
|`Esc`|Exit insert mode|
|`:w`|Save file|
|`:q`|Quit Vim|
|`:wq`|Save and quit|
|`:q!`|Quit without saving|
|`dd`|Delete a line|
|`yy`|Yank (copy) a line|
|`p`|Paste copied text|
|`/pattern`|Search for a pattern|
|`n`|Go to next search result|
|`N`|Go to previous search result|

### Advanced Vim Commands

|Command|Description|
|---|---|
|`:set number`|Show line numbers|
|`:set nonumber`|Hide line numbers|
|`:%s/old/new/g`|Replace all occurrences of `old` with `new`|
|`u`|Undo last change|
|`Ctrl + r`|Redo last change|
|`:split <file>`|Split window horizontally|
|`:vsplit <file>`|Split window vertically|
|`Ctrl + w + arrow`|Switch between split windows|

---

## Text Editing with Nano

### Basic Nano Commands

|Command|Description|
|---|---|
|`nano <file>`|Open a file in Nano|
|`Ctrl + O`|Save file|
|`Ctrl + X`|Exit Nano|
|`Ctrl + K`|Cut a line|
|`Ctrl + U`|Paste cut text|
|`Ctrl + W`|Search for text|
|`Ctrl + \`|Replace text|
|`Ctrl + G`|Open help menu|

---

## Advanced Commands

|Command|Description|
|---|---|
|`cron`|Schedule tasks|
|`crontab -e`|Edit cron jobs|
|`awk`|Text processing tool|
|`sed`|Stream editor for text manipulation|
|`rsync`|Sync files/directories|
|`dd`|Convert and copy files (e.g., create bootable USB)|
|`lsof`|List open files|
|`strace`|Trace system calls|
|`tmux`|Terminal multiplexer|
|`alias`|Create command shortcuts|