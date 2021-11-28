## Table of Contents
1. [Basics](https://github.com/p-arrow/Red-Blue-Guide/blob/main/OperatingSystem/Linux%20CLI.md#basics)
2. [Configuration](https://github.com/p-arrow/Red-Blue-Guide/blob/main/OperatingSystem/Linux%20CLI.md#configuration)
3. [Network](https://github.com/p-arrow/Red-Blue-Guide/blob/main/OperatingSystem/Linux%20CLI.md#network)
4. [Data / File](https://github.com/p-arrow/Red-Blue-Guide/blob/main/OperatingSystem/Linux%20CLI.md#data--file)
5. [Security / Encryption](https://github.com/p-arrow/Red-Blue-Guide/blob/main/OperatingSystem/Linux%20CLI.md#security--encryption)
6. [User / Groups](https://github.com/p-arrow/Red-Blue-Guide/blob/main/OperatingSystem/Linux%20CLI.md#user--groups)
7. [System](https://github.com/p-arrow/Red-Blue-Guide/blob/main/OperatingSystem/Linux%20CLI.md#system)

<br />

## Basics

- `[command1] && [command2]`: command2 gets executed when command1 finished successfully 
- `Alt + print-key`: Screenshot
- `Alt + "+"`: zoom in 
- **pwd**: print working directory
   - `pwdx [pid]`: print workig directory of process pid  
- **echo**: print function
- **xargs**: build and execute command lines from standard input
   - `find | xargs 2>/dev/null | base64 -d | strings -n 8`
   - `ls | while read line; do xargs $line; done`
- **man**: show manual   
   - `man [command]`
   - `/` = search within manual
   - jump through search result: `N` (forward)/`Shift+N` (backward)
- **ls**: show content of directory 
    - `ls -a` ("all")
    - `ls -l` ("long", more detailed)
- **cd**: change directory
    - `cd ..` (parent directory)
    - `cd ~` (Home directory)
    - `cd /` (root directory)
- `Strg + C`: abort process
- **exit**: finish terminal
- `mkfifo [Option] [Name]`: create named pipe (e.g. from terminal1 to terminal2)
- **date**: show date and time 
   - datum = $(date +%d.%m.%Y)
   - touch $(date +`hostname`-%d-%m-%y-%H%M.log)
- **cal**: calendar
- **background**
   - `[command] &`: perform [command] in background 

<br />

## Configuration

- **bashrc** (bash resource): script file that’s executed when a user logs in
   - `nano ~/.bashrc`: configure bashrc
- **$PS1**: (Primary) Prompt Shell Variable
   - Example: PS1="[\u@\h \w]$" --> [user@host working directory]$
- **$RANDOM**: Generates random integer between 0 and 32,767 each time it is referenced
   - `echo $(($RANDOM % 100))`: random number between 0 and 99
- **$PATH**: Indicates the search path for commands
- **$LANG**: Display Language
- **chsh**: change shell
   - (1) `cat /etc/shells` (check existing shells) 2) `chsh -s $(which zsh)` (switch shell)
   - `chsh -s /bin/rbash [username]` (apply restricted shell to user)
- **timedatectl**: show time settings 
   - `timedatectl list-timezones`
   - `timedatectl set-timezone`
   - `timedatectl set-local-RTC 0`
- **DEBIAN_FRONTEND**: environment variable for adjusting debconf (Debian package configuration system)
   - `DEBIAN_FRONTEND=noninteractive apt-get -y update`
   - `DEBIAN_FRONTEND=noninteractive apt-get -y upgrade`
   - `DEBIAN_FRONTEND=dialog` (default frontend for apt/apt-get )
   - `DEBIAN_FRONTEND=readline` (most traditional; for slow remote connections; entirely CLI)
- **tasksel**: a user interface for installing tasks
- **update-alternatives**: creates, removes, maintains and displays information about the symbolic links comprising the Debian alternatives system
   - `update-alternatives --query editor` to check the installed editors on the system
   - `update-alternatives --config x-session-manager` to choose a particular implementation of x-session-manager (e.g. GNOME, XFCE, Cinnamon etc.)

<br />

## Network

- **ifconfig**: show interface details
- **ip**:
   - `ip addr show`
   - `ip addr sh eth0`
   - `ip -br link show` (br=brief)
- **netstat**: print network connections
   - `netstat -tulpn`
- `ping [IPv4]`
- **host**: Identify host
   - `host [IPv4]`   
- **openssl**:
   - `openssl [command] -help`
   - `openssl s_client -connect host:port`
- **scp** (OpenSSH secure file copy):
   - `scp option source user@host:/tmp`
- **fuser**: find out process that opened specific port
   - `fuser 22/tcp`: SSH pid will be shown
- **ufw** (uncomplicated FW): managing netfilter FW
   - `ufw status`: show FW status
- **ipv6toolkit**:
   - `sudo scan6 -i [interface] -L -P local --print-unique -e`: get IPv6 + MAC
- **traceroute**: UDP probe by default (often ignored by FW though)
   - `traceroute -I example.com` (-I = send ICMP probe)
   - `traceroute -w 10 example.com` (-w = wait for response in seconds)
- **iperf**: check bandwidth between two \*nix machines

<br />

## Data / File

- `./[binary or script]`: execute binary/script
- **STDIN: 0 / STDOUT: 1 / STDERR: 2**
    - `2>&1`: redirect STDERR to STDOUT (">&" means "redirect file descriptor")
    - `&>/dev/null`: redirects all output to /dev/null
- **cp**: copy from to
    - `cp example.txt /var/www/html/`
- **less**: display data page by page
    - `q`: quit
- **head**: show first lines of file
- **tail**: show last lines of file
- **mv**: 1) Rename file 2) move file from to
    - `mv example1 example2`
    - `mv example1 /tmp`
- **rm**: remove file 
    - `rm *`: remove all data 
    - `rm -rf`: remove non-empty directory (-r=recursively, -f=no prompt)
- **mkdir**: create directory 
    - `mkdir -p lib/x86_64_so`: create directory including parent directory
    - `mkdir /home/newuser/{downloads,uploads}`: create several files at once
- **mktemp**: create file/directory with random name
    - `mktemp -d dir.XXXX` (-d = directory)
- **rmdir**: remove directory 
- **unlink**: to remove the specified file
    - `unlink [file]`
- **wc**: word count (lines / words / bytes / filename)
- **grep**: search for text file 
    - `-i`: ignore uppercase/lowercase 
    - `-v`: invers 
    - `-w`: treat serach term as word 
    - `-c`: return count of matching strings
    - `-l`: return name of files with matching lines
    - `-o`: print only matched parts (-o "s" ; -o "Word")
    - `ls | grep .pdf`
    - `grep 'word1\|word2'`: search two terms 
    - `grep "10\.1\.0\.10\," firewall.log | grep "23$"`
    - `grep -r -i "voldemort" /tmp/*.sh`
- **cat**: concatenate/read the file and output content
    - `cat -b dictionary.txt`
- **which** [data]: Returns path of linux file if existing in PATH
- **locate** [string]: Returns all paths that contain [string]
- **whereis** [data]: Returns all paths
- **find**:
    - `find ./dir1/dir2 -name *.txt`
    - `find . -readable -and -size 1033c`
    - `find ./dir -perm 664`
    - `find /usr/bin/ -perm 4000` Find files with suid functionality
    - `find . -empty`
    - `find -name '*.php' -mtime -1 -ls`
- **file**: display data-type info
- **zip/unzip**: for packages
- **tar**: Archive program for files and directories 
     - `tar -xvf [file]`: extract file verbosly
     - `tar -cvf [file] /etc`: create file verbosly 
     - `tar -cf archiv.tar test.txt test2.txt`: create archiv with files "test" and "test2"
     - `tar -xzvf archiv.tar.gz`: extract zipped file verbosly
- **sleep**: invoke delay (indicate in seconds)
- **wget**: non-interactive download of files from the Web
- **dd**: convert and copy a file (dd if=file_in of=file_out bs=1 skip=40)
- **pr** (print): Does formatting of files on the terminal screen or for a printer
- **touch**: 
    - `touch -a [filename]`: change access timestamp of file (-a access/-m modification time)
    - `touch -m [filename]`: change modification timestamp
    - `touch [filename1] -r [filename2]`: apply timestamp from f1 upon f2
    - `touch -h [filename]`: change timestamp of symbolic link (i/o original file)
    - `touch [filename]`: create file 
    - `touch -c [filename]`: do not create file if file does not exist yet (but only change timestamp)
    - `touch /tmp/'dir;bash -p'`: create file/dir + start bash 
- **ln**: create soft links
    - `ln -s [file] [name]`: soft link with self-created name
      - `-s` = soft link
      - hard links by default
- **sort**: Returns sorted text file 
    - `-r` = sorted reverse
    - `-n` = sorted in numerical order
    - `-k 2` = sorted acc. to second column
    - `-t ","` = delimited by ","
    - `sort -t "," -k 2 syslog.txt`
- **uniq**: retrieve unique elements from file
    - `uniq -u [word]`: -u = unique 
- **tr**: translate/squeeze/delete characters from stdin
    - `cat linux.txt | tr [a-z] [A-Z]`: Change text from lowercase to uppercase
    - `echo "TEXT" | tr a-zA-Z n-za-mN-ZA-M` (ROT13/Cäsar-Encryption)
    - `tr -s ' ' '\n'`: replace space with newline
- **nano**:
    - `Ctrl+Shift+6`: Mark block, then move cursor, then Strg+K to delete
    - `nano -l [file]`: show line number
- **vi**:
    - `i`: start insert Mode
    - `ESC`: exit insert Mode
    - In Normal mode: `y/y^/y$` (copy), `d/d$` (delete), `p` (paste)
    - `:wq` (write and quit)
    - `:wq!` (write, quit and override)
- **tee**: read from standard input and write to standard output and files
- **sed**: Stream Editor, useful with regex
    - `cat [file] | sed /regex-pattern/action`
    - action: d(elete), s(ubstitute), -n p(rint)
    - `cat /etc/passwd | sed '1,5s/sh/quiet/g'`
    - `cat /etc/passwd | sed -n '1,3p'`
    - `cat testing | sed '/^daemon/d'`
    - [Tutorialspoint](https://www.tutorialspoint.com/unix/unix-regular-expressions.htm)
- **cut**: Remove specific part of results
    - `cut -c5-5 syslog.txt`: returns 5th - 10th char. in each line
    - `cut -d " " -f1-4 syslog.txt`: returns first four entries of each line, delimited by " "
- **unshadow**:
    1. `sudo unshadow /etc/passwd /etc/shadow > password.txt`
    2. `john password.txt`
- **eog**: GNOME image viewer
    - `eog *` show all photos 

<br />

## Security / Encryption
- **md5sum**
- **sha256sum**
- **sha384sum**
- **sha512sum**
- **gpg**: built-in encryption/decryption tool (e.g. password vault, signature)
   - `gpg --full-gen-key`: generate keys + user ID
   - `gpg -c file`: encrypt with password (based on AES128 algorithm)
   - `gpg --encrypt file`: encrypt file with generated key + user ID
   - `gpg --decrypt file`: alternatively: gpg file.gpg
   - `gpg --keyserver pgp.mit.edu --search-keys example@email.com`: Search for public key online
   - `gpg --keyserver pgp.mit.edu --receive-keys 48BF6357AB80EB5E`: Get public key via key ID
   - `gpg --recipient example@email.net --armor --encrypt plain.txt`
      - `--armor` = for base64 encoded output
      - `--recipient` = to specify the public key
   - `gpg --keyserver pgp.mit.edu --send-keys [your fingerprint]`: Push your public key to keyserver 
   - `gpg --keyserver pgp.mit.edu --refresh-keys`: To check if your key has been successfully sent


<br />

## User / Groups
- **id**: show ID of user
    - `id [user]`
- **passwd**: Create new password for current user
- **smbpasswd**: Create new Samba password for current user
    - `smbpasswd -a [user]` (-a = add)
    - Ensure synchronization of Samba DB: smb.conf --> global setting --> `unix password sync` 
- **whoami**: Current User
- **users/who/w/finger**: Show Logged-In Users
- **su**: switch user account
    - `su [username]` 
- **chown**: change owner
    - `chown alice:alice document.docx`: user and group is "alice"
- **chgrp**: change group
- **chmod**: user (u), group (g) and others (o)
    - `chmod u=rw,g=rw,o=r [file name]`
    - `chmod o+w [file name]`
    - `chmod g-w [file name]`
    - `chmod 750 [file name]` (-rwx r-x ---)
    - `chmod 777 .` 
    - `chmod a=r [file name]` (set read-permission for "all")
    - `chmod 4755/u+s file` (set setuid)
    - `chmod 2755/g+s file` (set setguid)
    -  r/w/x | binary | octal
       ----- | ------ | -----
        ---  |  000   |   0
        --x  |  001   |   1
        -w-  |  010   |   2
        -wx  |  011   |   3
        r--  |  100   |   4
        r-x  |  101   |   5
        rw-  |  110   |   6
        rwx  |  111   |   7
- **useradd**: Adds accounts to the system
    - `useradd -d homedir -g groupname -m -s shell -u userid accountname`
    - `-m` = Creates the home directory if it doesn't exist
    - `-s /bin/false` = the new user would not be able to login via SSH
    - `useradd -d /home/mcmohd -g developers -s /bin/ksh mcmohd`
    -  The useradd command modifies /etc/passwd, /etc/shadow, and /etc/group (!)
- **usermod**: Modifies account attributes
    - `usermod -a -G sudo user` (appends user to group sudo)
- **userdel**: Deletes accounts from the system
    - `deluser user group` (deletes user from group)
- **groups**: list all groups of current user
- **groupadd**: Adds groups to the system
    - `groupadd [-g gid [-o]] [-r] [-f] groupname`
    - `-o` = with non-unique GID
    - `-r` =  add a system account
    - `-f` = exit if group already exists
- **groupmod**: Modifies group attributes
- **groupdel**: Removes groups from the system
- **chfn**: change user information
    - `chfn [user]`
    - `finger [user]`: Einträge von chfn anzeigen
- **history**: show command history of user
- **getent**: get entries from Name Service Libs
    - `getent group sudo`

## System
- **apt**
    - `apt update`
    - `apt upgrade`
    - More details: [BlueTeamOthers/Linux](https://github.com/p-arrow/Red-Blue-Guide/blob/main/Y_BlueTeamOthers/Linux.md#apt-package-tool) 
- **shutdown**
    - `shutdown -h now`: Instant shutdown
    - `shutdown -r now`: reboot
- **reboot**
- **Read-in CD**
    - `apt-cdrom add /media/cdrom`
- **service**:
    - `service [program] restart`: restart service (after config change)
    - `service [program] status`: show status 
    - `systemctl status [program]` 
- **nohup** (no hangup): continue command even user logs out or exit shell
- **journalctl**: query the systemd journal
    - `journalctl -xe`
- **lsusb**: display usb devices
- **lsmod**: display modules in the linux kernel
- **udevadm**: u(ser)dev(ice)adm(inistration) tool
    - `udevadm info -a -p /sys/bus/usb/devices/1-1`: -p = path, 1-1 = bus1 & port1
- **uname**: print system information (kernel etc.)
    - `uname -r`: distribution version
- **uptime**: show uptime
- **lsb_release**: print distribution specific information (lsb=Linux Standard Base)
    - `lsb_release -a`
- **df**: show disk space of complete file system
    - `df -ah`
- **du**: show disk usage of files in a directory
    - `du -ah /etc`
- **ps**: display information of active processes
    - VSZ (Virtual MemSize) / RSS (Physical MemSize) / TTY (?=none) / STAT (S=sleeping,R=running)
    - `ps -fp [pid]`: show corresponding CLI command 
    - `ps axu | grep [pid/program]`
    - `ps aux --sort=-%mem | less`
- **dmidecode**: displays the S(ystem)M(anagement)BIOS/DMI in a human-readable way
- **export**: show environment variables 
    - `export PS1='$ '` set PS1 to '$ '
- **modprobe**: remove/add modules
- **dmesg**: show Kernel-Ringpuffer
    - `dmesg -H` 
- **lspci**: Show PCI Devices
     - `lspci -vnn | grep [x]`: -vnn = verbose + PCI Vendor name & numbers
- **lsof**: Anzeige von belegten Ressourcen
    - `lsof -i :22` (SSH-Ressourcen)
    - `lsof -p [pid]`
    - `lsof -u [uid]`
    - `lsof -R [pid]` (show PPID)
- **free**: Memory Consumption
- **top**: Memory Consumption
- **kill**: send signal to process
    - `kill -l`: show all available signals
    - `kill -9 [PID]`
    - `sudo killall sshd`: kick out unauthorized user
- **htop**: GUI top
- **systemd**
- **pstree**: Show parent/child relation of processes
- **fsck**: file system check
- **cron**:
    - `* * * * * /usr/bin/echo.sh >> /dev/pts/0`: redirect output from echo.sh
    - `*/2 * * * * command`: execute command every two minutes
- **crontab**: 
    - `crontab -l`: display cron jobs
    - `crontab -u root -l`: user specific)
    - `grep -r [cronjob] /etc/cron.* /etc/crontab`
- **write**: send a message to another user on the host
- **mesg**: display (or do not display) messages from other users
- **jobs**: 	Lists all jobs
    - `bg %n`: 	Places job in background, where n is the job ID
    - `fg %n`: 	Brings job into foreground, where n is the job ID
    - `Control-Z`: Stops foreground job, places it in the background as stopped job
    - `[command] &`: "&" puts job in background
- **screen**: terminal multiplexer
    - `screen -S [name]`: start screen session with specified name
    - start task in window 0
    - `Ctrl+A, c`: create new window 1 (create another task)
    - `Ctrl+A, 0 or 1`: switch windows
    - `Ctrl+A,Shift+S`: split window horizontally
    - `Ctrl+A, Tab`: switch between split windows
    - `Ctrl+A, (|)`: split vertically
    - `Ctrl+A, Q`: Close all regions except the current one.
    - `Ctrl+A, X`: Close the current region.
- **grub**: boot loader
    - `grub-mkpasswd-pbkdf2`: protect GRUB from anyone who boots your computer
    - `cat /var/log/boot.log`: review boot log
    - `nano /etc/default/grub`: change grub settings (timeout etc.)