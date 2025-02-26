=====
Linux
=====

1. `Essential Commands`_
2. `System Commands`_
3. `Configurations`_
4. `Networking`_
5. `Storage`_
6. `Scripts`_
7. `Memory Management`_
8. `Kernel Development`_
9. `Kernel Data Structures`_
10. `Kernel Modules`_
11. `Device Drivers`_
12. `Linux From Scratch`_
13. `References & External Resources`_

`back to top <#linux>`_

Essential Commands
==================

* ``CTRL + ALT + F3``, virtual terminals

remote GUI login
----------------
    * can use VNC (Virtual Network Computing) server and client
    * allow RDP (Remote Desktop Protocol) for Windows user login

* ``ssh user@IP``, SSH remote login
* ``journalctl``, read system logs
* ``man COMMAND``, manual pages of COMMAND
* ``sudo mandb``, update manual pages
* ``apropos``, search commands
* ``ls -la``, list all files & dirs
* ``pwd``, print working dir
* ``cd``, change dir
* ``mkdir``, make new dir
* ``cp SOURCE DESTINATION``, copy files & dirs (with ``-r``)
* ``mv SOURCE DESTINATION``, move files & dirs
* ``rm``, delete files & dirs (with ``-r``)
* ``stat``, show file system status

Inode
-----
    * helps file systems keep track of data
    * contains metadata about a file

Hard Links
----------
    * ``ln TARGET_PATH LINK_PATH``
    * points to the same inode
    * if multiple hard links, deleting one will not delete the other
    * can only hard link to files on the same fs
    * need proper permissions to create and access

Soft Links
----------
    * ``ln -s TARGET_PATH LINK_PATH``
    * points to a path of the file or dir, like shortcut
    * ``readlink``, show full path of soft link
    * permissions only depend on the target
    * changing the target will break the link
    * can also create soft links with relative path

* ``chgrp GP FILE``, change group of file & dir
* ``chown``, change owner of file & dir, requires root
    * ``sudo chown user1:gp1 FILE``
* ``groups``, show user's groups
* most VMs require files to be less than 20GB
* ``chmod 6640``, change permissions of files & dirs
    * 4:r & SUID, 2:w & GID, 1:x & sticky, 0:- (can also use binary values)
    * permissions are checked linearly
    * ``u+rwx,g-x,o=r``
    * ``rws``, enabling SUID/GID causes file to be executed as the owner
    * ``rwS``, SUID/GID set but not executable
    * ``find . -perm /4000``, find files with SUID set with any permissions
    * ``rwt`` or ``rwT``, sticky bits are usually set on shared directories and allow the owner to
      remove the file no matter the permissions of the directory
* modified time: create or edit, change time: change metadata
* ``find``, find files & dirs
    * ``-mmin``, modified minute
    * ``-mtime``, modified days
    * ``-size``, -512k or +512k (c, k, M, G)
    * ``-perm 644`` (exact), ``-644`` (at least 644), ``/644`` (any of these)
    * ``-name -o -size``, OR
    * ``-not`` or ``\!``, NOT

View file content
-----------------
    * ``cat``, ``tac`` (reverse)
    * ``tail``, ``head`` (``-n 10`` for only 10 lines, ``-F`` for follow)
    * ``less``, ``more``

* ``sed -i 's/SEARCH/REPLACE/g' FILE``, edit file content
    * stream editor
    * replace only first without ``g``
    * show only preview without ``-i``
* ``cut -d ':' -f 2 FILE``, extract file content
* ``sort FILE``, sort content
* ``uniq FILE``, remove adjacent duplicates (better use with ``sort``)
* ``diff -c FILE1 FILE2``, show differences
    * ``sdiff`` or ``diff -y``, show side by side
* ``grep``, search text
    * ``-i``, ignore case
    * ``-r``, recursive through all files in a dir
    * ``-v``, invert match
    * ``-w``, only words
    * ``-o``, output only matching
    * use basic regular expressions, meta-characters lose special meaning, need to be escaped
    * ``egrep`` or ``grep -E``, use extended regular expressions, meta-characters do not need to be
      escaped

Regular Expressions
-------------------
    * ``^``, starts with
    * ``$``, ends with
    * ``.``, match any 1 character
    * ``\``, escape character
    * ``*``, match previous 0 or more times
    * ``+``, match previous 1 or more times
    * ``{X}``, previous can exist X times (``{MIN,MAX}``)
    * ``?``, previous can be optional
    * ``|``, match one or other
    * ``[]``, range (``[a-z]``) or sets (``[abc123]``)
    * ``[^]``, negated range or sets
    * ``()``, sub-expressions

* ``tar``, archive files into one tarball
    * tape archive
    * ``f FILE``, same as ``--file``
    * ``tf FILE.tar``, list contents from tar
    * ``cf FILE.tar FILE``, create tar
    * ``rf``, append/add to existing tar
    * ``xf``, extract (``xf FILE.tar -C DIR``, extract to 'DIR')
    * use ``sudo`` to preserve permissions

Compression and Decompression
-----------------------------
    * compress: ``gzip FILE``, ``bzip2``, ``xz``, ``zip FILE.zip FILE``
    * decompress: ``gunzip FILE.gz``, ``bunzip``, ``unxz``, ``unzip FILE.zip``
    * ``-k`` or ``--keep``, keep input files (delete by default)
    * ``-l`` or ``--list``, list contents
    * can also use ``less`` to list contents
    * only ``zip -r`` can compress multiple files or directories, others need to use ``tar`` first
    * archive and compress same time: ``tar czf FILE.tar.gz FILE``, ``tar cjf FILE.tar.bz2 FILE``,
      ``tar cJf FILE.tar.xz FILE``
    * auto detect: ``tar caf FILE.tar.gz/bz2/xz FILE``

* ``rsync -a SOURCE/ TARGET/``, sync two directories
    * remote synchronization
    * syncing to remote server need SSH daemon
* ``sudo dd if=INPUT of=OUTPUT bs=BLOCK_SIZE status=progress``, disk imaging
    * should unmount the disk first to avoid changes
    * reverse ``if`` and ``of`` to restore
    * should not use in VMs

Redirect input/output
---------------------
    * ``<``, stdin (0)
    * ``1>`` or ``>``, stdout
    * ``2>``, stderr
    * ``>>``, append
    * ``> FILE.txt 2>&1`` or ``> FILE.txt >&``, both stdout and stderr
    * ``<<EOF ...any text here... > EOF``, heredoc
    * ``<<< string``, here string

* ``command1 | command2``, pipe 'COMMAND1' output to 'COMMAND2'
* ``column``, arrange columns

`back to top <#linux>`_

System Commands
===============


Reboot & Shutdown
-----------------
    * ``reboot`` or ``systemctl reboot`` and ``shutdown`` or ``systemctl shutdown``
    * both are links to ``systemctl``
    * can use ``--force`` or ``--force --force``
    * ``shutdown 13:00``, schedule shutdown at 1pm
    * ``shutdown +20``, schedule shutdown in 20 minutes
    * ``shutdown -r +20``, schedule reboot in 20 minutes
    * ``shutdown -r +2 'Show message to users'``

* ``systemctl get-default``, list boot target
* ``systemctl set-default multi-user.target``, set new default boot target to be without GUI
* ``systemctl isolate graphical.target``, change to GUI target without needing to reboot but will
  not set default

Boot Targets
------------
    * ``graphical``
    * ``multi-user``: text based
    * ``emergency``: read-only root file system
    * ``rescue``: more programs than ``emergency``, but less than ``multi-user``
    * root user password must be set to use ``emergency`` and ``rescue``

Bootloader
----------
    * purpose is to start the Linux kernel
    * GRUB (Grand Unified Bootloader) is a popular one
    * **on BIOS**
        - ``grub2-mkconfig -o /boot/grub2/grub.cfg``, make config
        - bootloader should be installed on first section of the block device
        - ``lsblk``, list block devices
        - ``grub2-install /dev/sda``, install GRUB in first section
    * **on EFI**
        - ``grub2-mkconfig -o /boot/efi/EFI/fedora/grub.cfg``, make config on EFI
        - ``dnf reinstall grub2-efi grub2-efi-modules shim``, auto place config file in right
          location
    * edit the file ``/etc/default/grub`` and use above commands to update grub config

init
----
    * initialisation system start up the system as necessary
    * **Units**
        - text files with instructions to start the system
        - can be service, socket, device, timer or others
    * **Service Units**
        - what command to use to start a program
        - what to do when a program crashes and restarts
        - tell the ``init`` how to manage lifecycle of applications
        - ``man systemd.service``, list instructions that can be added in services
        - ``systemctl edit --full sshd.service``, edit a service
        - ``systemctl revert sshd.service``, restore to default
        - ``systemctl status sshd.service``, check service status
        - ``systemctl start sshd.service``, start the service
        - ``systemctl stop sshd.service``, stop the service
        - ``systemctl restart sshd.service``, restart the service
        - ``systemctl reload sshd.service``, reload the service without closing
        - ``systemctl reload-or-restart sshd.service``, use restart if reload is not supported
        - ``systemctl disable sshd.service``, do not start the service on startup
        - ``systemctl enable sshd.service``, start the service on startup
        - ``systemctl is-enabled sshd.service``, check if service will start on startup
        - ``systemctl enable --now sshd.service``, start the service on startup and start it now
        - ``systemctl disable --now sshd.service``, do no start the service on startup and stop
          it now
        - ``systemctl mask atd.service``, do not allow the service to be started by others
        - ``systemctl unmask atd.service``, allow the service to be started by others
        - ``systemctl list-units --type service --all``, list all services available

Processes
---------
    * ``top``, list processes in real time
        - order by cpu usage
    * ``ps``, list processes at the time the command is run
        - only show current processes in session by default
        - ``ps -aux``, Unix style
        - ``ps aux``, BSD style
        - processes in '[]' are kernel processes
        - ``ps 1``, list process by PID
        - ``ps -U user``, list processes started by 'user'
        - ``pgrep -a bash``, search process by name
        - ``ps l``, include nice value column
        - ``ps fax``, list processes tree
    * ``nice -n 9 bash``, start process with specific nice value (-20 to 19)
        - processes inherit nice values
        - regular user can only assign values between 0 and 19
        - assigning negative nice value requires root
    * ``renice 1 PID``, change process nice value
        - can only lower the value once as regular user
    * ``kill -SIGHUP PID``, send signal to process by name
        - ``kill PID``, send ``TERM`` signal by default
        - ``kill -L``, list signals list
        - ``kill -9 PID``, send signal by number
        - ``pkill -KILL bash``, kill processes that are bash
    * ``CTRL + c``, breaks the process
    * ``CTRL + z``, pause the process and sends it to background
    * ``bg 1``, run paused background process
    * ``sleep 100 &``, run process in background
    * ``fg``, bring back background/paused process
    * ``jobs``, list background/paused processes
    * ``lsof -p PID``, list files used by the process
        - ``lsof /var/log/``, list which processes use the files

Logs
----
    * logging daemons collect, organize and store logs
    * ``rsyslog``, stores logs in ``/var/log``
        - rocket-fast system for log processing
    * ``journalctl $(which sudo)``, show logs generated by ``sudo``
        - ``journalctl -u sshd.service``, logs by service
        - ``journalctl -f``, follow mode
        - ``journalctl -p err``, show only error logs (``info``, ``warning``, ``err``, ``crit``)
        - ``journalctl -g '^a'``, using with grep expressions
        - ``journalctl -S 02:00``, show only logs after 2am
        - ``journalctl -S 02:00 -U 03:00``, show only logs between 2am and 3am
        - ``journalctl -S '1999-1-1 12:00:59``, using dates
        - ``journalctl -b 0``, current boot logs
        - ``journalctl -b 1``, previous boot logs (require ``/var/log/journal``)
    * ``log``, list login history
        - ``lastlog``, show last login time

Schedule Jobs
-------------
    * ``anacron``, schedule tasks specified in days
        * for machines that are not running 24 hours a day
        * can also schedule by editing ``/etc/anacrontab``
        * ``anacron -T``, verify anacron syntax
        * ``anacron -n``, run commands now
    * ``crontab``, schedule tasks even in minutes
        - ``* * * * * user command``, (minute, hour, day, month, day of week)
        - ``*`` for all values
        - ``,`` for multiple values
        - ``-`` for range of values
        - ``/`` for specific steps
        - can omit ``user``
        - can also schedule by creating files in ``/etc/cron.*`` directories
        - ``etc/crontab``, systemwide cron
    * ``at``, run command at specified time
        - ``at 03:00``, ``at '03:00 January 1 1999``, ``at 'now + 30 minutes'``
        - ``CTRL + d`` to save
        - ``atq``, list jobs
        - ``at -c JOB_ID``, show job description
        - ``atrm JOB_ID``, remove job

dnf
---
    * ``dnf repolist``, show enabled repositories list
    * ``dnf repolist --all``, show all repositories list
    * ``dnf config-manager --enable REPO_ID``, enable repository
    * ``dnf config-manager --disable REPO_ID``, disable repository
    * ``dnf config-manager --add-repo REPO_URL``, add a repository
    * remove the file from ``Repo-filename`` output by ``repolist -v``
    * ``dnf search 'PKG'``, search for a package
    * ``dnf group list``, list groups
    * ``dnf group install 'GROUP_NAME'``, install packages from group
    * ``dnf install ./app.rpm``, install from rpm file
    * ``dnf autoremove``, remove hanging dependencies
    * ``dnf provides docker``, identify which package provides the app
    * ``dnf repoquery -l moby-engine``, list which files are in the package

* ``df``, check file system usage
    * ``df -h``, show in human-readable form
* ``du -sh /``, check size of directory
* ``free -h``, check memory usage
* ``uptime``, check cpu usage
* ``lscpu``, check cpu usage in detail
* ``lspci``, check other hardware usage

Checking file system
--------------------
    * must be unmounted before checking
    * ``xfs_repair -v /dev/sda1``, repair XFS file system
    * ``fsck.ext4 -v -f -p /dev/sda1``, check ext4 file system

* ``systemctl list-dependencies``, check services running or not

Kernel runtime parameters
-------------------------
    * ``sysctl -a``, list all kernel runtime parameters
    * ``sysctl -w runtime.para.name=1``, set parameter value (non persistent)
    * add files in ``/etc/sysctl.d/*.conf``, persistent change
    * ``sysctl -p /etc/sysctl.d/custom.conf``, read value from file

SELinux
-------
    * Security Enhanced Linux, a security module
    * enabled by default, allow fine grain control
    * **SELinux context label**
        - ``ls -Z``, list context labels for files
        - ``unconfined_u:object_r:user_home_t:s0``, 'user:role:type:level'
        - ``ps axZ``, list context labels for processes
        - processes with ``unconfined_t`` domain are running unrestricted
        - ``id -Z``, list context for current user
        - ``semanage login -l``, check user mapping to SELinux 'user'
        - ``semanage user -l``, check users mapping to SELinux 'users'
    * has policy configuration
    * every user logged in is mapped to SELinux 'user'
    * each 'user' can only assume predefined 'roles'
    * 'type' restrict what an object can do, called 'domain' on processes
    * 'level' is never used on regular systems, only used in enterprises
    * ``getenforce``, check SELinux enabled or not (``Enforcing``, ``Permissive``, ``Disabled``)
    * benefits
        - only certain users can enter certain roles and types
        - lets authorized users and processes do their job
        - authorized users and processes are allowed to take only a limited set of actions
        - everything else is denied

PIE
---
    * Position-Independent Executables, can be loaded anywhere in memory
    * ASLR: Address Space Layout Randomisation, security feature that mitigates some attacks
      based on fixed addresses
    * without PIE, ASLR can be applied for shared libraries, but not for executables
    * SSP: Stack Smashing Protection, to ensure the parameter stack is not corrupted
    * stack corruption can alter the return address of a function, transferring control to
      malicious code

`back to top <#linux>`_

Configurations
==============


Users
-----
    * Shadow package contains programs for handling passwords in a secure way
    * check ``doc/HOWTO`` in Shadow source directory for full explanation
    * when using Shadow support, programs that verify passwords, such as display managers, FTP
      programs, pop3 daemons must be Shadow-compliant
    * ``pwconv``: enable shadowed passwords
    * ``grpconv``: enable shadowed group passwords
    * ``useradd`` by default creates user and group with same name, and IDs start at 1000
    * ``useradd newUser``: add new user
    * ``useradd -D`` or ``/etc/login.defs``: list defaults
    * ``passwd newUser``: set password for user
    * ``userdel newUser``: delete user but ``/home`` directory will not be deleted
    * ``userdel -r newUser``: delete user and ``/home`` directory
    * ``useradd -s /bin/shell1 -d /home/dir1 newUser``: change default shell and home directory
    * ``useradd -u 1111 newUser``: set user ID
    * ``/etc/passwd``: file contains user details
    * ``id``: list users and ID
    * ``useradd --system sysAcc``: create system account
    * system accounts has ID less than 1000, and are for programs, used by daemons, and no
      ``/home`` is created
    * ``usermod -d /home/dir1 -m user1``: change user home directory
    * ``usermod -l user2 user1``: change user name
    * ``usermod -L user1``: lock user account (will be able to login if ssh with key is setup)
    * ``usermod -U user1``: unlock user account
    * ``usermod -e 2049-1-1 user1``: set expiration date for user account (year-month-day)
    * ``usermod -e "" user1``: remove expiration date for user account
    * ``chage -d 0 user1``: set expiration date for user password (user must change password)
    * ``chage -d -1 user1``: remove expiration for user password
    * ``chage -M 30 user1``: user must change password every month
    * ``chage -M -1 user1``: user password never expires
    * ``chage --list user1``: list passwords expiration dates

Groups
------
    * certain groups allow root privileges (e.g ``wheel``)
    * **Primary group**
        - also called Login group
        - a program runs with the same privileges as the user's primary group
        - files created will be owned by the user and the primary group
    * ``groupadd newGroup``, add new group
    * ``gpasswd -a newUser newGroup``, add user to group
    * ``groups newUser``, list groups 'newUser' belong to
    * ``gpasswd -d newUser newGroup``, remove user from group
    * ``usermod -g newGroup newUser``, change user's primary group
    * ``groupmod -n oldGroup newGroup``, change group name
    * ``groupdel newGroup``, delete group (cannot delete if group is user's primary)

ENV variables
-------------
    * ``printenv`` or ``env``, list environment variables
    * ``echo $HOME``, print an environment variable value
    * can edit ``.bashrc`` file to set variables
    * place scripts in ``/etc/profile.d/`` to be executed at login
    * place files in ``/etc/skel/`` to have default files for new users in their ``/home``
    * edit ``$PATH`` variable to add more paths

Resource Limits
---------------
    * edit ``/etc/security/limits.conf`` to limit users resources
    * ``ulimit -a``, list current user limits
    * users can only lower limits by default
    * users can raise to hard limit only once

Privileges
----------
    * users part of ``wheel`` group are allowed to run commands as root
    * ``sudo gpasswd -a user1 wheel``, add user to ``wheel`` group
    * ``/etc/sudoers`` defines who can use which commands with ``sudo``, never edit directly
    * ``sudo visudo``, edit ``/etc/sudoers`` file safely
    * ``sudo -u user1 ls``, run as commands 'user1'
    * ``sudo -iu user1``, login as user1
    * ``sudo -i`` or ``su -l`` or ``su -``, login as root
    * ``sudo passwd root``, set new password for root

PAM
---
    * Pluggable Authentication Module can configure methods to authenticate users
    * ``/etc/pam.d/``, configuration files for PAM
    * modules are loaded in order, but control field can change the order

ACLs
----
    * define specific permissions to two or more users/groups
    * ``setfacl --modify user:user2:rw FILE``, 'user2' can access while other non-owners can't
    * files with ACL will have ``+`` when ``ls -l``
    * ``getfacl FILE``, check for ACL
        - ``mask`` defines maximum permissions file/directory can have, useful to limit
          existing permissions
        - ``setfacl --modify mask:r FILE``
    * ``setfacl --modify group:gp1:rw FILE``, set ACL for 'gp1'
    * ``setfacl --modify user:user2:--- FILE``, deny all permissions for 'user2'
    * ``setfacl --remove user:user2 FILE``, remove ACL for 'user2'
    * ``setfacl --remove group:gp1 FILE``, remove ACL for 'gp1'
    * ``setfacl --recursive -m user:user2:rwx DIR1/``, define ACL recursively
    * ``setfacl --recursive --remove user:user2 DIR1/``, remove ACL recursively

Attributes
----------
    * can make file or directory behave differently
    * ``chattr +a FILE``, can only append
    * ``chattr -a FILE``, remove append only attribute
    * ``chattr +i FILE``, file is immutable, even root user cannot modify
    * ``lsattr FILE``, list attributes of file
    * ``man chattr``, list all available attributes

Disk quotas
-----------
    * can use ``quota`` to set quotas
    * can limit storage and how may files and directories can be created
    * for ext file system
        - ``quotacheck --create-files --user --group /dev/sdb2``, create files to track usage
        - ``quotaon /mnt/``, turn on quota if mounted on ``/mnt/``
    * for xfs file system
        - can edit ``/etc/fstab`` to have ``defaults,usrquota,grpquota``
        - ``quota --user user1``, list quotas for 'user1'
        - ``edquota --user user1``, edit quotas for 'user1'
        - ``edquota --group gp1``, edit quotas for 'gp1'
        - 1 block is 1KB
        - ``fallocate --lenght 100M FILE1``, create 100MB file to test quota
        - allowed to exceed soft limit for specific days, ``grace period``
        - set inodes limit to limit files and directories
        - ``quota --edit-period``, edit grace period

`back to top <#linux>`_

Networking
==========

* ``ip l`` or ``ip a``, list network interfaces
* ``ip r``, list route table
* ``/etc/resolv.conf``, dns resolver file
* ``/etc/sysconfig/network-scripts/``, system configure network according to files in the it
* ``nmtui`` or ``nmcli``, to edit network configurations
    * ``nmcli device reapply eno1``, apply changes without reboot
    * ``nmcli connection show``, list configured connections
    * ``nmncli connection modify MyWifi autoconnect yes``, configure device to auto connect
    * **Connecting to wifi**

        .. code-block:: bash

           nmcli device wifi list --rescan yes
           nmcli device wifi connect SSID password "PASSWORD"
           ncmli connection show
           nmcli connection down CURRENT_CONNECTION
           nmcli connection up NEW_CONNECTION


* ``/etc/hosts``, edit the file for static host names
* ``systemctl status NetworkManager.service``, daemon that starts network settings and devices
* ``ss -tunlp`` or ``netstat -tunlp``, list listening connections

Firewall
--------
    * ``firewall-cmd --get-default-zone``, list default zone
    * ``firewall-cmd --set-default-zone=public``, set default zone
    * ``firewall-cmd --list-all``, list current firewall rules
    * ``firewall-cmd --info-service=ssh``, list service details
    * ``firewall-cmd --add-service=http`` or ``firewall-cmd --add-port=80/tcp``, allow traffic
    * ``firewall-cmd --remove-service=http`` or ``firewall-cmd --remove-port=80/tcp``, remove
    * ``firewall-cmd --get-active-zones``, list active zones
    * ``firewall-cmd --add-source=10.11.12.0/24 --zone=trusted``, allow traffic based on IP
    * ``firewall-cmd --remove-source=10.11.12.0/24 --zone=trusted``, remove from zone
    * ``firewall-cmd --runtime-to-permanent`` or ``firewall-cmd --add-port=80/tcp --permanent``

Static Routing
--------------
    * ``ip route add 192.168.0.0/24 via 10.0.0.100 dev eno3``, add new route
    * ``ip route del 192.168.0.0/24``, delete route
    * ``ip route add default via 10.0.0.100``, add default gateway
    * ``ip route del default via 10.0.0.100``, delete default gateway
    * ``nmcli connection modify en03 +ipv4.routes "192.168.0.0/24 10.0.0.100"``, route permanent
    * ``nmcli connection modify en03 -ipv4.routes "192.168.0.0/24 10.0.0.100"``, remove route
    * ``nmcli device reapply eno3``, apply changes

Synchronize time using network
------------------------------
    * ``chronyd.service`` updates system clock periodically
    * ``timedatectl``, list current timezone
    * ``timedatectl set-timezone Region/City``, set timezone
    * ``timedatectl list-timezones``, list timezones
    * ``systemctl set-ntp true``, activate NTP service

Bind
----
    * ``bind``, including ``bind-utils``, is popular for hosting dns server
    * ``/etc/named.conf``, configuration file
    * ``systemctl start named.service``, start bind
    * ``firewall-cmd --add-service=dns --permanent``, allow connection to dns service
    * ``dig @localhost google.com``, check bind working or not
    * zone: group dns data for specific domain
    * ``/var/named/``, contains example zone files

Email
-----
    * incoming emails are saved in ``/var/spool/mail/`` directory
    * ``postfix`` is widely used to setup mail server
    * can add new aliases in ``/etc/aliases``
    * **IMAPS**
        - Internet Message Access Protocol over SSL (early IMAP does not encrypt)
        - can use ``dovecot`` daemon to setup IMAPS

SSH
---
    * listens on port ``22`` by default
    * ``/etc/ssh/ssh_config``, client configuration file
    * ``/etc/ssh/sshd_config``, server configuration file
    * edit files in ``/etc/ssh/ssh_config.d/`` and ``/etc/ssh/sshd_config.d/`` to prevent reset
    * ``$HOME/.ssh/config``, file to specify users and IP (use ``600`` permission)
    * ``ssh-keygen``, generate ssh key pairs which are stored in ``$HOME/.ssh/``
    * ``ssh-copy-id user@server``, copy the public key to the server
    * can also manually edit ``$HOME/.ssh/authorized_keys`` to copy public key
    * it is better to generate ssh key pairs on the client, so that only public key has to
      be copied over the Internet
    * ``ssh-keygen -R IP``, remove old finger prints from ``known_hosts``

HTTP Proxy
----------
    * can use ``squid`` daemon to setup http proxy server
    * ``firewall-cmd --add-service=squid --permanent``, allow connection to squid
    * edit ``etc/squid/squid.conf`` for configuration

HTTP Server
-----------
    * Apache ``httpd`` daemon is widely used with ``mod_ssl``
    * ``firewall-cmd --add-service=http`` and ``firewall-cmd --add-service=https``
    * configuration files are in ``/etc/httpd/``
    * ``/etc/httpd/conf/httpd.conf``, primary configuration file
    * ``apachectl configtest``, check configuration
    * ``/etc/httpd/conf.d/ssl.conf``, default ssl configuration
    * most modules are auto enabled when installed
    * ``/var/log/httpd/``, default log directory
    * logging is done by ``log_config_module``
    * it is recommended to separate log files by each host
    * can restrict access by editing ``Options Indexes FollowSymLinks``, ``Require all granted``
    * ``sudo htpasswd -c /etc/httpd/passwords user1``, create hashed password file for user1
    * generated password file can be used for authentication with ``AuthType``, ``AuthBasicProvider``,
      ``AuthName``, ``AuthUserFile`` and ``Require`` options

Database Server
---------------
    * ``mariadb``, a fork of mysql, can be used to setup database server
    * ``firewall-cmd --add-service=mysql --permanent``, open firewall if needed
    * ``mysql_secure_installation``, setup to secure the database
    * ``/etc/my.cnf.d/mariadb-server.cnf``, main configuration file

`back to top <#linux>`_

Storage
=======

* ``lsblk``, list block devices
    * ``TYPE: part``, partition of a disk
* ``sudo fdisk --list /dev/sda``, list partitions of a device
* ``sudo cfdisk /dev/sda``, edit disk partition table interactively

Swap
----
    * ``swapon --show``, check swap usage
    * ``sudo mkswap /dev/sdb3``, prepare the partition
    * ``sudo swapon --verbose /dev/sdb3``, use partition as swap
    * ``sudo swapoff /dev/sdb3``, stop using partition as swap
    * use file as swap
        - ``sudo dd if=/dev/zero of=/swap bs=1M count=128 status=progress``, prepare the file
        - ``sudo chmod 600 /swap``
        - ``sudo mkswap /swap``
        - ``sudo swapon --verbose /swap``

File systems
------------
    * file system needs to be created before a partition can be used
    * ``sudo mkfs.xfs /dev/sdb1``, create xfs file system
        - ``sudo xfs_admin``, modify xfs file system
    * ``sudo mkfs.ext4 /dev/sdb1``, create ext4 file system
        - ``sudo tune2fs -l /dev/sdb1``, modify ext based file system
    * ``sudo mount /dev/sdb1 /mnt/``, mount a file system
    * ``sudo umount /mnt/``, unmount a file system
    * ``/etc/fstab``, file that instructs which file systems to be mounted automatically
        - use UUID instead of device names
        - ``sudo blkid /dev/sdb1``, check UUID
    * **on demand mounting**
        - only mount when needed, useful when using remote servers
        - ``autofs`` daemon can be used, usually with ``nfs-utils``
        - edit ``/etc/exports`` for network sharing
        - edit ``/etc/auto.master`` to configure ``autofs``
    * ``sudo mount -o ro,noexec,nosuid /dev/sdb1 /mnt``, mount file system with specific options
    * ``sudo mount -o remount,ro /dev/sdb1 /mnt``, remount file system with new options
    * it is better to do ``umount`` and ``mount`` again with new options

* ``findmnt``, find file systems and mount points
    * ``findmnt -t xfs,ext4``, show only xfs and ext4

LVM
---
    * Logical Volume Manager, to create virtual block devices
    * can represent separate physical devices as one continuous partition
    * PV: physical volume, VG: volume group, LV: logical volume, PE: physical extent
    * ``lvmdiskscan``, list available PV
    * ``pvcreate /dev/sdb /dev/sdd``, create pv to be used by LVM
    * ``pvs``, list current attached PV
    * ``vgcreate vg1 /dev/sdb /dev/sdd``, add PV to VG
    * ``vgextend vg1 /dev/sde``, add new PV to existing VG
    * ``vgreduce vg1 /dev/sde``, remove PV from VG
    * ``pvremove /dev/sde``, remove PV
    * ``lvcreate --size 8GB --name partition1 vg1``, create LV from existing VG
    * ``lvs``, list LV
    * ``vgs``, list VG
    * data in LVM is divided into multiple PEs
    * ``lvresize --extents 100%VG vg1/partition1``, resize LV
    * ``lvresize --size 8G vg1/partition1``, shrink LV
    * ``lvdisplay``, information about LV
    * ``mkfs.xfs /dev/vg1/partition1``, create file system on LV
    * ``lvresize --resizefs --size 3G vg1/partition1``, resize both LV and file system

* ``cryptsetup``, encrypt storage device
    * encrypted disks can be found in ``/dev/mapper/``, and is same as regular disk
        - ``mkfs.xfs /dev/mapper/mysecuredisk``
        - ``mount /dev/mapper/mysecuredisk /mnt``, can be mounted
    * **plain**
        - takes password and encrypt all data with it
        - ``cryptsetup --verify-passphrase open --type plain /dev/sda mysecuredisk``, can read
          decrypted data
        - ``cryptsetup close mysecuredisk``, can only read encrypted data
        - changing password requires encrypting all data again
    * **LUKS**
        - more user friendly to setup, default mode
        - ``cryptsetup luksFormat /dev/sda``
        - ``cryptsetup luksChangeKey``, change encryption key
        - ``cryptsetup open /dev/sda mysecuredisk``, can read decrypted data
        - ``cryptsetup close mysecuredisk``, can only read encrypted data

RAID
----
    * Redundant Array of Independent Disks, combine multiple storage devices into single storage
    * unlike from LVM, RAID provides any options for redundancy or parity
    * **Level 0**
        - striped array, not redundant
        - disks are groups and Linux sees them as single storage
        - total usable storage equals to sum of total devices
        - data on entire array will be lost just by losing one disk
    * **Level 1**
        - mirrored array
        - when writing data to one disk, the same data is written to all disks
    * **Level 5**
        - require minimum of 3 disks, can lose up to one disk
        - keep parity, information to rebuild data, on each disk
    * **Level 6**
        - require minimum of 4 disks, can lose up to two disks
    * **Level 10/ RAID 1+0**
        - has advantages of both Level 0 and 1
    * ``mdadm --create /dev/md0 --level=0 --raid-devices=3 /dev/sda /dev/sdb /dev/sdc``
    * ``mkfs.ext4 /dev/md0``, can create file system
    * ``mdadm --stop /dev/md0``, deactivate array
    * when reboot, Linux scans for superblock on devices to auto rebuild the array
    * ``mdadm --zero-superblock /dev/sda /dev/sdb /dev/sdc``, not to rebuild the array
    * ``mdam --create /dev/md0 --level=1 --raid-devices /dev/sda /dev/sdb --spare-devices=1 /dev/sdc``,
      will auto add `/dev/sdc` if one of the disks fails
    * ``mdam --manage /dev/md0 --add /dev/sdc``, add new disk to the array
    * ``mdam --manage /dev/md0 --remove /dev/sdc``, remove disk from the array
    * ``/proc/mdstat``, file contains information about RAID

`back to top <#linux>`_

Scripts
=======

* ``#!/bin/bash``, 'shebang' should always be the first line of every script
* commands in the script are the same as commands written in terminals
* ``chmod +x myscript.sh``, scripts must be executable
* ``/full/path/to/myscript.sh`` or ``./script.sh``, run the script
* ``help``, list bash built-ins
    * ``help if``, print each built-in

.. code-block:: sh

   #!/bin/bash
   
   # this is a comment
   
   date >> /tmp/script.log
   cat /proc/version >> /tmp/script.log
   echo hello >> /tmp/script.log
   
   if test -f /tmp/archive.tar.gz; then
       mv /tmp/archive.tar.gz /tmp/archive/tar.gz.OLD
       tar acf /tmp/archive.tar.gz /etc/dnf/
   else
       tar acf /tmp/archive/tar.gz /etc/dnf/
   fi


`back to top <#linux>`_

Memory Management
=================

* `Virtual Memory`_, `Memory Management Unit`_, `Kernel Virtual Memory`_
* `Kernel Logical Addresses`_, `Kernel Virtual Addresses`_, `User Virtual Addresses`_
* `User-Space Allocation`_

Virtual Memory
--------------
    * system that uses an address mapping, virtual address space to physical address space
    * Physical Address: used by hardware, e.g. DMA (Direct Memory Access), peripherals
    * Virtual Address: used by software, e.g. Load/Store instructions (RISC), any instruction
      accessing memory (CISC)
    * maps to physical RAM, and hardware devices like PCI devices, GPU RAM, On-SoC IP blocks
    * each process can have different memory mapping, making one process's RAM inaccessible to
      others
    * kernel RAM is invisible to user-space processes
    * memory can be moved, or swapped to disk
    * kernel can map hardware device memory into a process's address space
    * Shared Memory: physical RAM mapped into multiple processes at once
    * read, write, execute permissions can be set on memory regions
    * virtual memory mapping is assisted by hardware, MMU
    * no penalty for permissions and performance when accessing already-mapped regions
    * same CPU instructions are used for accessing RAM and mapped hardware
    * usually software will only use virtual addresses

Memory Management Unit
----------------------
    * MMU is a hardware responsible for virtual memory mapping
    * between CPU and memory, often part of CPU, but separate from memory controller
    * handles all memory accesses from Load/Store instructions, permissions, and
      exception/page fault on invalid access
    * **Translation Lookaside Buffer**
        - TLB is part of MMU system, a hardware buffer with a list of mappings from virtual to
          physical address space, and permission bits
        - depending on CPU, TLB has a fixed number of entries
        - MMU checks the TLB when CPU accesses a virtual address
        - a page fault exception is generated, and the CPU interrupted when virtual address is
          not in the TLB, or insufficient permissions
        - virtually contiguous regions do not have to be physically contiguous
    * **Page**
        - basic units of memory, page size varies by architecture
        - Common Sizes: ARM 4k, ARM64 4k or 64k, MIPS configurable, x86 4k
        - architectures with configurable sizes are configured at kernel build time
        - Page Frame: page-sized and page-aligned physical memory block
        - a mapping often covers multiple pages
    * **Page Fault**
        - CPU exception generated when software attempts to use invalid virtual address
        - can be caused by address not mapped, insufficient permissions, or valid address but
          swapped out
    * **Page Table**
        - kernel data structure to store the mappings, e.g. ``struct_mm``, ``vm_area_struct``
        - CPU generate a page fault for some valid mappings that are not in TLB
        - page fault handler find the relevant mapping in page tables, select and remove
          existing TLB entry, create a new TLB entry, and return to user space process
    * **Swapping**
        - MMU enabling kernel to swap frames to disk, and remove its TLB entry to free up RAM
        - the frame can be reused by another process
        - when page fault, kernel put the process to sleep, copy the frame from disk into
          unused frame in RAM, fix the page table entry, and wake the process
        - a page is not necessarily restored to the same physical frame, but has the same
          virtual address to hide the difference from user-space process

Kernel Virtual Memory
---------------------
    * Linux kernel and user-space processes both use virtual address
    * Virtual Address Splitting: upper part for kernel, and lower part for user space
    * **On 32-bit System**
        - by default, the split is at ``0xC0000000``, ``CONFIG_PAGE_OFFSET``
        - kernel uses the top 1GB, and each user-space process gets the lower 3GB of virtual
          address space
        - configurable at kernel build time, ``CONFIG_VMSPLIT_*`` option
        - in addition to 1GB, ~104MB is reserved at the top of kernel's memory space for
          non-contiguous allocations, such as space used by ``vmalloc()``
    * **On 64-bit System**
        - split varies by architecture
        - ``0x8000000000000000`` for ARM64, and ``0xffff880000000000`` for x86_64
    * **3 Types of Virtual Addresses**
        - Kernel: Kernel Logical Address, Kernel Virtual Address
        - User-Space: User Virtual Address

Kernel Logical Addresses
------------------------
    * kernel normal address space, includes memory from ``kmalloc()`` and most allocation methods,
      and kernel stacks of each process
    * can never be swapped out
    * **Fixed Mapping**
        - fixed mapping between virtual and physical, converting from one another is easy
        - e.g. Virt: ``0xC000000`` -> Phys: ``0x00000000``
        - use ``__pad(x)`` and ``__va(x)`` macros for conversion
        - virtually contiguous regions are also physically contiguous
    * for fixed mapping and non-swappable, kernel logical addresses are suitable for DMA
      transfers
    * **For Small-Memory System**
        - systems with less than 1GB of RAM
        - kernel logical address space is from ``PAGE_OFFSET`` to the end of physical memory
    * **For Large-Memory System**
        - systems with more than 1GB of RAM
        - on 32-bit systems, only bottom part of physical RAM is mapped directly into kernel
          logical address space
        - 64-bit systems have enough kernel address space to accommodate all the RAM

Kernel Virtual Addresses
------------------------
    * addresses above kernel logical address mapping, can also called ``vmalloc()`` area
    * for non-contiguous mappings, ``vmalloc()``, and memory-mapped I/O, ``ioremap()``, ``kmap()``
    * physically non-contiguous, easier to allocate, but unsuitable for DMA

User Virtual Addresses
----------------------
    * used by user-space programs, all addresses below ``PAGE_OFFSET``
    * mapping per process, threads share a mapping, and complex behaviour with ``clone()``
    * only used portions of RAM are mapped
    * memory can be swapped out and moved, and is not contiguous
    * user buffers are not suitable for kernel use or DMA
    * mapping is changed at context switch time
    * same virtual addresses in two different processes will likely be used to map different
      physical addresses
    * **Shared Memory**
        - implemented with MMU, map the same physical frame into two different processes
        - virtual addresses do not need to be same
    * **Lazy Allocation**
        - a performance optimisation, kernel waiting to allocate pages requested by a process
          until the pages are actually used
        - kernel creates a request record in its page tables, and return to the process
          without updating the TLB
        - page fault is generated when the newly allocated memory is touched
        - kernel validate the mapping, allocate a physical page frame, update the TLB, and
          return from the page fault handler for the user-space program to resume
        - user-space program is never aware of the page fault
        - pre-fault pages at the start of execution for time-sensitive processes, e.g. ``mlock()``

User-Space Allocation
---------------------
    * **mmap()**
        - standard way to allocate large amount of memory, often used for files
        - ``MAP_ANONYMOUS`` flag to allocate normal memory for the process
        - ``MAP_SHARED`` flag to share allocated pages with other processes
    * **brk()/sbrk()**
        - ``brk()``: sets the top of the program break, in effect increases the heap size
        - ``sbrk()``: increases the program break
    * ``malloc()`` and ``calloc()`` will use ``brk()`` or ``mmap()`` depending on the allocation size

`back to top <#linux>`_

Kernel Development
==================

* `Build Tools`_, `Kernel Dependent Tools`_, `Kernel Config`_, `Build Kernel`_, `Install Kernel`_
* `Upgrade Kernel`_, `Customise Kernel`_, `Find Drivers`_, `Common Config`_, `Code Navigation`_
* never build the kernel with root permissions enabled, and never do kernel development under
  ``/usr/src/`` directory
* multi-lib system requires compiling applications for both 32-bit and 64-bit
* the source code has stable and development rc branches, and is available at [kernel.org](https://kernel.org/)

Build Tools
-----------
    * check ``Documentation/Changes`` to verify tools versions
    * **Compiler**
        - ``gcc`` C compiler is needed as the kernel is written in C
        - do not use the most recent gcc version to build the kernel, ``gcc --version``
    * **Linker**
        - tools from ``binutils`` to link and assemble source files, ``ld -v``
    * **Make**
        - scan the kernel source tree for file that need to be compiled
        - recommended to use the latest stable version, ``make --version``

Kernel Dependent Tools
----------------------
    * some packages need to be upgraded to work properly with the new kernel version
    * **util-linux**
        - collection of small utilities, mostly to manipulate disk partitions and hardware
          clock in the system
        - recommended for the latest version to support new features, ``fdformat --version``
    * **module-init-tools**
        - required to use kernel modules
        - recommended for the latest version to support new features, ``depmod -V``
    * **File System Tools**
        - ``e2fsprogs``: to manage ext2, ext3, and ext4 file systems, recommended latest version,
          ``tune2fs``
        - ``jfsutils``: to use IBM JFS file system, ``fsck.jfs -V``
        - ``reiserfsprogs``: to use ReiserFS file system, ``reiserfsck -V``
        - ``xfsprogs``: to use XFS file system from SGI, ``xfs_db -V``
        - ``quota-tools``: to use quota functionality of the kernel, ``quota -V``
        - ``nfs-utils``: to use NFS file system, ``showmount --version``
    * **udev**
        - enable persistent and dynamic device-naming system in ``/dev``
        - almost all Linux distributions use ``udev`` to manage ``/dev`` directory
        - recommended to use latest or distribution provided version, ``udevadm -V``
    * **procps**
        - include commonly used tools such as ``ps`` and ``top``, ``ps --version``
    * **pcmciautils**
        - user-space helper program to properly use PCMCIA devices
        - recommended for the latest version, ``pccardctl -V``

Kernel Config
-------------
    * configure kernel options with ``make config`` by choosing every options, or can be based on
      a pre-built configuration
    * ``.config`` file is generated in the top directory
    * **Default Config**
        - based on defaults by kernel maintainer, ``make defconfig``
        - usually the configuration the maintainer use for personal machines
    * **Interactive Config Tool**
        - Menuconfig: terminal based, ``make menuconfig``
        - Gconfig: GTK+ based
        - Xconfig: QT based
    * check advanced build options in ``Documentation/kbuild`` directory

Build Kernel
------------
    * after setting config options, use ``make`` to build the kernel
    * can specify directory of the built files with ``make O=/dir/for/output``, config file also
      need to be placed in that directory, `make O=/dir/for/output defconfig`
    * **Multithread Build**
        - ``make -jX`` with X being twice the number of processors in the system
        - ``make -j`` without value will create a new thread for every subdirectory
    * **Partial Build**
        - can build a specific subdirectory or a single file within the kernel tree
        - e.g. ``make drivers/usb/serial`` will build the files in that directory, but will not
          build the final module images
        - use ``make M=drivers/usb/serial`` to build all necessary files and link the final
          module images
        - run ``make`` again to affect the changes in the subdirectory to the final kernel image
        - use ``make drivers/usb/serial/visor.ko`` to build specific file, and do final link to
          create the module
    * **Cross Compliation**
        - using more powerful machine to build for a smaller embedded system
        - specify architecture with ``ARCH=``, C compiler with ``CC=``, and cross-compile
          toolchain with ``CORSS_COMPILE=``
        - ``make ARCH=arm CROSS_COMPILE=/usr/local/bin/arm-linux`` to build with ARM toolchain
        - ``make CC="ccache gcc"`` or ``make CC="ccache distcc"`` to change compiler for the build
          system

Install Kernel
--------------
    * **Auto Install**
        - can use distro based ``installkernel`` to auto install a built kernel and modify
          bootloader
        - use ``make modules_install`` to install if there are any modules built, and install the
          main kernel with ``make install``
        - module files will be placed in ``/lib/modules/KERNEL_VERSION``
        - during installation, kernel is verified, static kernel is placed in ``/boot``, required
          initial ramdisk images are created, and bootloader is updated
    * **Manual Install**
        - install modules with ``make modules_install``, and run ``make kernelversion`` to know
          the version
        - run ``cp arch/x86/boot/bzImage /boot/bzImage-KERNEL_VERSION``, and
          ``cp System.map /boot/System.map-KERNEL_VERSION`` to copy static kernel image
        - modify the bootloader, such as GRUB, LILO
        - can run ``info grub`` to get more info

Upgrade Kernel
--------------
    * can update the kernel while retaining configurations, always backup ``.config`` file
    * get the source code to upgrade, reconfigure it based on the previous kernel config, build
      and install the new kernel
    * **Kernel Patch**
        - Stable Patches: apply to the base kernel version, e.g. 2.6.17.9 patch only apply to
          2.6.17 release
        - Base Patches: apply to the previous base kernel version, e.g. 2.6.18 patch only
          apply to 2.6.17
    * use ``patch -p1 < PATCH_FILE`` in the kernel directory to apply the patch
    * check the ``Makefile`` or run ``make kernelversion`` to check if the patch is applied
    * to upgrade more than two versions, downgrade and upgrade to desired version save steps,
      e.g. go from 2.6.17.9 to 2.6.17, and then upgrade to 2.6.17.11
    * use ``make oldconfig`` and ``make silentoldconfig`` to update new configurations
    * upgrades between stable releases rarely have new configuration options

Customise Kernel
----------------
    * can check distribution's kernel configuration to know which modules are necessary
    * distribution kernel files can be found under ``/usr/src``, and config file can be found at
      ``/proc/config.gz`` or ``/boot/config-$(uname -r)``
    * only disable options that are certain not needed
    * **sysfs**
        - virtual file system with symlinks to all around the file system, should always be
          mounted at ``/sys``
        - internal structure usually changes due to reorganisation of devices
    * **Device Discovery**
        - need to find proper ``sysfs`` class device that the device is bound to
        - e.g. use ``basename $(readlink /sys/class/net/eth0/device/driver/module)`` to trace
          through ``sysfs`` tree to find out which module is controlling it
        - in the kernel source, use ``find -type f -name Makefile | xargs grep MODULE_NAME`` to
          find the config options for the module
        - any output from the find command that has ``CONFIG_`` need to be enabled to build the
          module

Find Drivers
------------
    * all kernel modules have internal list of devices they support that is auto generated
      by the list of devices the driver tells the kernel it supports
    * easiest way find which driver control which device is to build all the drivers and let
      ``udev`` startup process match the driver to the device
    * **for PCI Devices**
        - known by vendor ID and device ID, use ``lspci`` to list all PCI devices in the format
          ``<BUS_ID:DEVICE_ID.FUNCTION_ID> <CLASS>: <VENDOR> <DEVICE> (rev REVISION_IN_HEX)``
        - check ``/sys/bus/pci/devices/0000:<BUS_ID:DEVICE_ID.FUNCTION_ID>`` for ``vendor`` and
          ``device`` files
        - in kernel source directory, search vendor definition with
          ``grep -i <VENDOR_ID> include/linux/pci_ids.h``, and driver source files with
          ``grep -Rl <VENDOR_DEF> *``
        - PCI drivers contain a list of supported devices in ``struct pci_device_id``, and match
          the vendor and device ID to check if it supports
        - search the Makefiles with ``find -type f -name Makefile | xargs grep DRIVER_NAME`` for
          ``CONFIG_`` to build the driver
    * **for USB Devices**
        - use ``lsusb`` to list USB devices in the format of
          ``Bus <BUS_ID> Device <DEVICE_ID>: ID <VENDOR_ID>:<PRODUCT_ID> <VENDOR> <PRODUCT>``
        - USB device numbers change every time it is plugged in, only vendor and product ID
          are unique
        - in kernel source directory, search with ``grep -i -R -l <VENDOR_ID> drivers/*`` for
          vendor definition
        - USB drivers have a list of supported devices in ``struct usb_device_id``
        - search the Makefiles with ``find -type f -name Makefile | xargs grep DRIVER_NAME`` for
          ``CONFIG_`` to build the driver
    * **for Root File System**
        - contain all initial programs, and usually entire system config
        - the kernel must be able to find the root file system at boot time
        - recommended to build file system for root partition, and disk controller for the disk,
          and use ramdisk image at boot time
        - determine the file system type with ``mount | grep " / "``, and check block devices with
          ``tree -d /sys/block | egrep "hd|sd"``
        - disk partitions are numbered, but main block devices are not
        - the whole main block device must be configured to access the individual partitions
        - go up a chain of devices through the symlink of ``/sys/block/<BLOCK_DEVICE>``, and find
          the necessary drivers
    * **for Disk Controller**
        - e.g. ``/sys/block/sdb`` will symlink to some ``/sys/devices/<SOMETHING_LONG>/block/sdb``
        - go up the link and take note of the disk controller driver in
          ``/sys/devices/<SOMETHING_LESS_LONG>/target0:0:0/0:0:0:0``
        - go up and find another driver, e.g. ``/sys/devices/pci<SOMETHING>/0000:<SOMETHING>``
    * enable the necessary file system type driver, and disk controller drivers in kernel config

Common Config
-------------
    * **Disks**
        - most USB storage devices need SCSI subsystem, SCSI disk support, and USB storage
          support
        - IDE disks need PCI support, IDE subsystem, IDE support, generic IDE controller for
          ATA system, and PCI IDE controllers
        - SATA disks use ``libata``, and need PCI support, SCSI subsystem, SCSI disk support,
          SCSI low-level drivers of specific SATA controller type
    * **CD-ROM**
        - IDE CD-ROM drives need the same as IDE disks, and additional IDE CD-ROM support
        - SCSI and SATA CD-ROM  drives need the same as SATA or SCSI disks, and additional
          SCSI CD-ROM support
    * **Devices**
        - USB controllers need PCI support, USB support, USB host controllers, and specific
          USB device driver
        - IEEE 1394 FireWire need PCI support, IEEE 1394 support, specific FireWire host
          controller and device support
        - PCI hotplug such as ExpressCard need PCI support, PCI hotplug support, ACPI
          controller for most types, and PCI express controller
        - PCMCIA and CardBus need PCI support, PCCARD support, PCMCIA or CardBus device
          support, and card bridge support
        - Sound system need basic sound support, ALSA support, base ALSA options, and specific
          sound device support
    * **CPU**
        - Processor need sub-architecture type, and processor family type, can use Generic
          architecture options to run on all types of machines
        - for multicore CPUs, SMP option should be enabled
        - Preemption model can be changed, such as using main internal kernel locks
        - can enable kernel Suspend mode, with the option to specify resume or not
        - CPU frequency scaling need basic frequency scaling support, frequency governors with
          one default based on the processor type
        - for 32-bit Intel CPU, there are three different memory models
        - ACPI need ACPI support, and specific drivers to control ACPI devices
    * **Networking**
        - all network options need main Networking support, with TCP/IP option
        - Netfilter need Network packet filtering, with Netfilter netlink interface and
          Xtables support, and protocols to filter
        - Ethernet need PCI support, basic network device support,and specific device drivers
        - IrDA need IrDA subsystem, IrDA protocols, and IrDA device support
        - Bluetooth need Bluetooth subsystem, protocols, and specific device drivers
        - Wireless network need IEEE 802.11 option, protocols, and PCI or USB device driver
          options
    * **File System**
        - hardware RAID is handled by the disk controller without help from the kernel
        - software RAID need Multiple devices driver support, RAID support with at least one
          RAID configuration
        - LVM need Multiple devices driver support, Device mapper support with helper modules
        - SMB, CIFS, and OCFS2 file systems need respective system support
    * **Security**
        - different security models and default linux capabilities should be enabled
        - SELinux need network option, Auditing support, Socket and Networking Security Hooks,
          and NSA SELinux support, along with various SELinux options
    * **Debugging**
        - timestamp options can be enabled on kernel messages
        - Magic SysRq key can trigger different actions, check
          ``Documentation/admin-guide/sysrq.rst`` in kernel source for more information
        - debugfs can be enabled and mounted at ``/sys/kernel/debug`` directory
        - enabling many different kernel debugging options may help kernel developers, but
          decrease performance

Code Navigation
---------------
    * to use with cscope and ctags, run ``make tags`` and ``make cscope``
    * to use with LSP, run ``python scripts/clang-tools/gen_compile_commands.py``, pass ``--help``
      argument for more info

`back to top <#linux>`_

Kernel Data Structures
======================

* `File Operations`_

File Operations
---------------
    * ``struct file_operations`` defined in ``<linux/fs.h>``
    * a collection of function pointers, and set up the connection between device numbers and
      device driver's operations
    * mostly implement system calls such as ``open``, ``read``, etc., and unsupported operations
      must be left `NULL`
    * each open file is an object associated with its own methods through ``f_op`` pointer, OOP
      concept
    * ``char __user *`` in the methods means a pointer to user-space address that cannot be
      directly dereferenced
    * ``__user`` has no effect for normal compilation, but can be used to find misuse of
      user-space addresses
    * **``struct module *owner``**
        - pointer to the module that owns the structure
        - used to prevent the module being unloaded while operations are in use
        - almost always initialised to ``THIS_MODULE`` in ``<linux/module.h>``
    * **``loff_t (*llseek) (struct file *, loff_t, int)``**
        - to change current read write position in a file
        - ``loff_t``: long offset with at least 64 bits wide even on 32-bit platforms
        - NULL method will modify the position counter in the ``file`` structure
        - positive return value for success, and negative on errors
    * **``ssize_t (*read) (struct file *, char __user *, size_t, loff_t *)``**
        - to retrieve data from the device, NULL method will fail with ``-EINVAL``
        - return number of bytes read on success
        - called by read(2)
    * **``ssize_t (*read_iter) (struct kiocb *, struct iov_iter *)``**
        - possibly async read with ``iov_iter`` as source
    * **``ssize_t (*write) (struct file *, const char __user *, size_t, loff_t *)``**
        - send data to the device, ``-EINVAL`` for NULL method
        - return number of bytes written on success
        - called by write(2)
    * **``int (*iterate_shared) (struct file *, struct dir_context *)``**
        - to read directory contents, should be NULL method for device files
    * **``__poll_t (*poll) (struct file *, struct poll_table_struct *)``**
        - check activity on the file, and can go to sleep until I/O
        - return a bit mask to indicate non-blocking read/write is possible
        - NULL method means the device is readable and writable without blocking
        - called by select(2) and poll(2)
    * **``long (*unlocked_ioctl) (struct file *, unsinged int, unsinged long)``**
        - to call device-specific commands
        - called by ioctl(2)
    * **``int (*mmap) (struct file *, struct vm_area_struct *)``**
        - to request a mapping of device memory to a process address space
        - NULL method will return ``-ENODEV``
        - called by mmap(2)
    * **``int (*open) (struct inode *, struct file *)``**
        - to open an inode, and create a new ``struct file``, driver is not required to declare
          the method
        - good place to initialise ``file->private_data`` to point to a device structure
        - NULL method always succeed opening the device, but driver is not notified
    * **``int (*flush) (struct file *, fl_ownder_t id)``**
        - to close a process copy of a file descriptor for a device, used in very few drivers
        - should wait for any outstanding device operations
        - NULL method will ignore the user application request
        - called by close(2)
    * **``int (*release) (struct inode *, struct file *)``**
        - called when the last reference to an open file is closed, can be NULL like ``open()``
    * **``int (*fsync) (struct file *, loff_t, loff_t, int datasync)``**
        - to flush any pending data, NULL method return ``-EINVAL``
        - called by fsync(2)
    * **``int (*fasync) (int, struct file *, int)``**
        - to notify the device of ``FASYNC`` flag change
        - can be NULL method if the driver does not support async notification
        - called by fcntl(2)
    * **``int (*lock) (struct file *, int, struct file_lock *)``**
        - for file locking, necessary for regular files but not required by device drivers
        - called by fcntl(2) for ``F_GETLK, F_SETLK, F_SETLKW`` commands
    * **``unsigned long (*get_unmapped_area) (struct file *, unsigned long, unsinged long, unsinged long, unsinged long)``**
        - to find a location in process address space to map in a device memory, usually done
          by memory management code
        - drivers can implement to enforce alignment requirements, most drivers can have NULL
          method
        - called by mmap(2)
    * **``int (*check_flags) (int)``**
        - allow a module to check the flags passed to fcntl
        - called by fcntl(2) for ``F_SETFL`` command

`back to top <#linux>`_

Kernel Modules
==============

* `Kernel Role`_, `Device and Module Classes`_, `Protection Levels`_, `Compile and Load Modules`_
* `Current Process`_, `Module Stacking`_, `Module Conventions`_, `Module Functions`_, `Module Parameters`_

Kernel Role
-----------
    * **Process Management**
        - create, destroy, and handle processes I/O connection
        - process communication can be through signals, pipes, or interprocess communication
        - the scheduler is part of process management
    * **Memory Management**
        - kernel builds virtual address space on limited available resources
        - different parts of kernel interact with memory management subsystem
    * **File System**
        - kernel builds structured file system on top of unstructured hardware
    * **Device Control**
        - almost every system operation maps to a physical device
        - device drivers perform any device control operations
    * **Networking**
        - network operations must be managed by the OS as they are not specific to a process
        - routing and address resolution issues are implemented in the kernel

Device and Module Classes
-------------------------
    * **Module**
        - functionality that can be added or removed from a kernel at runtime
        - a module is an object code that can be dynamically linked with ``insmod`` or unlinked
          with ``rmmod``
        - a module is linked to the kernel, and can only call functions exported by the kernel
        - each module implements char module, block module or network module
        - every kernel module is event-driven
        - a module's initialisation function terminates immediately, and exit function is
          called just before it is unloaded
        - the exit function must undo everything the init function did
    * **Character Device**
        - can be accessed as a stream of bytes, and by means of file system nodes, e.g.
          ``/dev/tty1``
        - char driver implements at least ``open``, ``close``, ``read``, and ``write`` system calls
        - most char devices can only be accessed sequentially, cannot move back and forth like
          in regular file
        - but char devices that look like data areas, such as frame grabbers, use ``mmap()`` or
          ``lseek()``
    * **Block Device**
        - can host a file system, and accessed by file system nodes in ``/dev``
        - Linux allows read and write to a block device like a char device, by permitting the
          transfer of any number of bytes at a time
        - block and char devices differ only how data is managed internally, and block drivers
          have different interface to the kernel than char drivers
    * **Network Interface**
        - a network driver only handles data packets without connection details
        - a network device is not stream-oriented, and the interface is not mapped to a node
          in the file system
        - communication for kernel and network driver is different from char and block drivers
    * file system type is just a software driver, as it maps low-level data structures to
      high-level ones, and is independent of actual data transfer to and from the disk

Protection Levels
-----------------
    * modern CPUs enforces protection of system software from applications, and have at least
      two protection levels
    * in processors with several levels, such as x86, the highest, kernel space or supervisor
      mode, and lowest, user-space, levels are used
    * each mode can have its own address space or memory mapping
    * execution transfers from user-space to kernel space whenever an application makes a
      system call or is suspended by hardware interrupt
    * interrupt handling code is running in the process context, async, and not related to any
      particular process

Current Process
---------------
    * the process that invokes the system call, global item defined in ``<asm/current.h>``
    * ``current`` is a pointer to ``struct task_struct`` defined by ``<linux/sched.h>``
    * can access ``current->pid``, ``current->comm``, and others

Compile and Load Modules
------------------------
    * can check ``Documentation/kbuild`` directory in kernel source for detailed information
    * ensure correct versions of compiler, module utilities, and necessary tools
    * a module need to be recompiled for each version of the kernel that it is linked to
    * need to use macros and ``#ifdef`` to make module code work with multiple kernel versions
    * **Makefile**
        - ``obj-m := module.o`` is to build a module from ``module.o`` object file
        - add ``module-objs := file1.o file2.o`` if ``module.ko`` needs to be generated from two
          source files
    * invoke ``make`` command within the context of kernel build system with
      ``make -C KDIR M=MODULE_SOURCE modules``
    * **insmod**
        - load modules, and can assign module parameter values before linking to the kernel
        - correctly designed module can be configured at load time
        - can fail with unresolved symbols
    * **modprobe**
        - check if a module references symbols that are not currently defined in the kernel
        - look for other modules that define the symbols, and load them into the kernel
    * **rmmod**
        - remove modules, fail if the module is in use, or kernel disallow module removal
        - kernel can be configured to allow forced removal of modules
    * **lsmod**
        - list currently loaded modules by reading the ``/proc/modules`` virtual file
        - also provide additional information such as modules using other modules
        - currently loaded modules can also be found in ``/sys/module``

Module Stacking
---------------
    * other modules using the exported symbols, useful in complex projects
    * symbols exported by the loaded module become part of the kernel symbol table
    * one ``modprobe`` command can sometimes replace several invocations of ``insmod``
    * stacking can split modules into multiple layers and reduce development time
    * use ``EXPORT_SYMBOL`` and ``EXPORT_SYMBOL_GPL`` macros to export symbols for other modules,
      the latter macro makes the symbol available to GPL-licensed modules only

Module Conventions
------------------
    * Mechanism: what capabilities are provided, Policy: how provided capabilities can be used
    * write kernel code to access the hardware, but do not force policies on user, and avoid
      having security policy in the code
    * a driver only makes hardware available, how it is used to applications should be ignored
    * make policy only when necessary, e.g. digital I/O driver may only offer byte-wide access
    * different drivers can offer different capabilities for the same device
    * decomposition, different module for each new functionality, allows scalability and
      extendability
    * kernel code must be reentrant, running in more than one context at the same time
    * the kernel stack can be as small as a single 4096 byte page
    * never declare large automatic variables, use dynamic allocation at call time if necessary
    * kernel API functions starting with double underscore are generally low-level component,
      and should be used with caution
    * kernel code cannot do floating point arithmetic, as it would need the kernel to save and
      restore the floating point processor's state on entry and exit from kernel space
    * **Headers**
        - most kernel code includes a large number of header files
        - about all module code has ``<linux/module.h>``, for symbols and functions needed by
          modules, and ``<linux/init.h>``, to specify initialisation and cleanup functions
        - ``<linux/sched.h>`` contains definitions of kernel API used by drivers
        - most also include ``<linux/moduleparam.h>`` to enable parameter passing to the module at
          load time
    * should specify which licence with ``MODULE_LICENSE``
    * definitions should be put at the end of the file, e.g. ``MODULE_AUTHOR``,
      ``MODULE_DESCRIPTION``, ``MODULE_VERSION``, ``MODULE_ALIAS``, ``MODULE_DEVICE_TABLE``
    * use standard C tagged structure initialisation syntax for portability, and compact code

Module Functions
----------------
    * arguments passed to kernel registration functions are usually pointers
    * most registration functions are prefixed with ``register_``
    * **Initialisation**
        - initialisation functions should be declared ``static``, as they are not meant to be
          visible outside specific file
        - ``__init`` tells the kernel that the function is used only at initialisation time, and
          is dropped after the module is loaded
        - can use ``__initdata`` for data used only during initialisation

        .. code-block:: c

           static int __init init_function(void) {}
   
           module_init(init_function);


    * **Cleanup**
        - every module requires a cleanup function, which unregisters interfaces and returns
          all resources to the system before the module is removed
        - functions with ``__exit`` can be called only at module unload or system shutdown time
        - if module is built into kernel, or kernel disallow module unloading, functions with
          ``__exit`` are discarded
        - a module without a cleanup function cannot be unloaded

        .. code-block:: c

           static void __exit cleanup_function(void) {}
   
           module_exit(cleanup_function);


    * **Error Handling**
        - module code must always check return values
        - module should continue to provide any capabilities it can after failing to register
        - modules that fail to load must undo any registration operations performed before
          the failure by itself, since there is no per-module registry
        - the kernel will be in unstable state if the module does not unregister after failure
        - ``goto`` statements are useful for error recovery, but can be difficult to manage for
          complex cases
        - can also track registered values, and call cleanup function in case of any error
        - error codes are negative and defined in ``<linux/errno.h>``
        - it is customary to unregister in reverse order used to register
        - cleanup function cannot be marked ``__exit`` when it is called by nonexit code
        - kernel will call the module before initialisation is completed
        - the code should be able to be called after first registration
        - it is possible that the kernel is using a registered facility, but the module's
          initialisation function fails after registering that facility

        .. code-block:: c

           int stuff_ok;
   
           int __init init(void)
           {
               int err = -ENOMEM;
   
               item1 = allocate(arg1);
               item2 = allocate(arg2);
   
               if (!item1 || !item2)
                   goto fail;
   
               err = register_stuff(item1, item2);
               if (!err)
                   stuff_ok = 1;
               else
                   goto fail;
               return 0;
           fail:
               cleanup();
               return err;
           }
   
           void cleanup(void)
           {
               if (item1)
                   release_thing(item1);
               if (item2)
                   release_thing(item2);
               if (stuff_ok)
                   unregister_stuff();
               return
           }



Module Parameters
-----------------
    * parameter values can be assigned at load time by ``insmod`` or ``modprobe``
    * module must make parameters available by declaring with ``module_param()`` macro defined in
      ``<linux/moduleparam.h>``
    * can also use ``module_param_array()`` to supply comma-separated list
    * all module parameters should have a default value
    * **Parameter Permission**
        - use definitions from ``<linux/stat.h>`` for permission value in ``module_param()``
        - permission value controls access to representation of the module parameter in ``sysfs``
        - no ``sysfs`` entry for 0 permission, and appears under ``/sys/module`` otherwise
        - ``S_IRUGO`` allows read by everyone but cannot change
        - ``S_IRUGO | S_IWUSR`` allows root to change
        - parameters should not be writable if there are no capabilities to detect the change
          and react accordingly
    * **Available Types**
        - byte, hexint, short, ushort, int, uint, long, ulong
        - charp: a character pointer
        - bool: a bool, values 0/1, y/n, Y/N.
        - invbool: the above, only sense-reversed (N = true).

`back to top <#linux>`_

Device Drivers
==============

* `Driver Components`_, `User-Space Drivers`_, `Scull Driver`_

Driver Components
-----------------
    * driver code should register device number first
    * **Device Numbers**
        - char devices are identified by ``c`` in the first column of ``ls -l /dev`` output, and block
          devices by ``b``
        - ``ls -l /dev`` output also shows major and minor device number in the form of ``MAJOR, MINOR``
        - major number shows the driver associated with the device, and minor number is used by the
          kernel to determine which device is being referred to
        - multiple drivers can share major numbers, but mostly one major to one driver
        - can get a direct pointer to the device from the kernel, or use the minor number as
          an index into a local array of devices
        - 32-bit ``dev_t`` type in ``<linux/types.h>`` has 12 bits for major and 20 bits for minor
        - use ``MAJOR(dev_t dev)``, ``MINOR(dev_t dev)``, and ``MKDEV(int major, int minor)``
        - need to get one or more device numbers when setting up a char device
        - use ``register_chrdev_region()`` defined in ``<linux/fs.h>`` if device numbers are known
        - new drivers should use dynamic allocation of major device number with
          ``alloc_chrdev_region()``
        - always free the device numbers with ``unregister_chrdev_region()``, usually in the
          module cleanup function
        - common device numbers can be found in ``Documentation/admin-guide/devices.txt``
    * most driver operations need ``file_operations``, ``file``, and ``inode`` kernel data structures

User-Space Drivers
------------------
    * useful when dealing with new and unusual hardware
    * **Advantages**
        - full C library can be linked in, and the driver can do tasks without using external
          programs
        - can debug without going through a running kernel
        - user-space driver error will not harm the entire system, and can be stopped if it
          hangs
        - since user memory is swappable, the driver will not occupy RAM if it is not in use
        - well-designed user-space driver can allow concurrent access to a device
        - easier to write a closed-source user-space driver
        - user-space driver usually implements a server process, and client applications can
          connect to it to perform actual communication with the device, e.g. X server
    * **Disadvantages**
        - no interrupts, most system calls are limited to privileged user
        - direct memory access is only by mmapping ``/dev/mem``
        - I/O ports access is only after calling ``ioperm`` or ``iopl``, which not all platforms
          support, and access to ``/dev/port`` can be slow
        - context switch makes response time slower, and swap makes it worse

scull Driver
------------
    * Simple Character Utility for Loading Localities, char driver in LDD 3rd Edition
    * acts on a memory area as though it were a device, hardware independent and portable
    * global memory area: data within the device is shared by all file descriptors that
      opened it
    * persistent memory area: data is not lost if the device is closed and reopened
    * ``scullX`` devices contains global and persistent memory area
    * ``scullpipeX`` are FIFO devices
    * ``scullsingle`` allows only one process at a time to use
    * ``scullpriv`` is private to each virtual console
    * ``sculluid`` and ``scullwuid`` can be opened multiple times, but only one user at a time

`back to top <#linux>`_

Linux From Scratch
==================

* `Preparations`_, `Building`_, `Directories`_, `Backup and Restore`_
* this section is mostly from following along the Linux From Scratch book

Preparations
------------
    * **Partitioning**
        - use 10GB partition for minimal system, or 30GB for further enhancements
        - to create unallocated space from allocated free space, booting into live Linux and
          resizing with parted or gparted can be easy
        - use fdisk or cfdisk to create a partition from the unallocated free space
        - create a file system with ``mkfs -v -t ext4 /dev/<name>``
        - for the host system to access, mount the partition to the directory where LFS system
          will be built
        - the partition must be mounted on every host system restart, or edit ``/etc/fstab`` to
          auto mount
    * **Download Files**
        - required files can be found at https://www.linuxfromscratch.org/mirrors.html#files
        - store the files in the directory where LFS partition is mounted
        - file ownership may need to be modified with ``chown root:root /LFS/dir/files/*``
    * **Directories**
        - create ``etc``, ``var``, ``usr/bin``, ``usr/lib``, ``usr/sbin``, along with symlinks of ``usr/*``
          in root directory of LFS system
    * **lfs User**
        - create group and user named ``lfs`` with ``groupadd`` and ``useradd``, and set password
          with ``passwd lfs``
        - change ownership of files in LFS system directory to ``lfs`` user
        - can start a shell of ``lfs`` user with ``su - lfs``
    * **Environment Setup for lfs User**
        - create ``.bash_profile`` with ``exec env -i HOME=$HOME TERM=$TERM /bin/bash`` to set
          empty environment
        - create ``.bashrc`` with following commands:
        - ``set +h``: turns off bash hash function to search ``PATH`` on every program run and use
          newly compiled tools
        - ``umask 022``: set user file-creation mask to make newly files and directories are
          only writable by their owner, but readable and executable by anyone
        - set env variable of ``LFS=original_mount_point``
        - ``LC_ALL=POSIX`` to control localisation of certain programs
        - ``LFS_TGT=$(uname -m)-lfs-linux-gnu`` for non-default machine description
        - ``PATH=/usr/bin`` and ``if [ ! -L /bin ]; then PATH=/bin:$PATH; fi`` to add ``/bin`` to
          ``PATH`` if it is not symlinked
        - ``PATH=$LFS/tools/bin:$PATH`` to use the new cross-compiler
        - ``CONFIG_SITE=$LFS/user/share/config.site`` to prevent configure scripts loading
          configurations from the host
        - ``MAKEFLAGS=-j$(nproc)`` to let ``make`` use all cores for faster build jobs
        - env variables need to be exported using ``export``

Building
--------
    * offset or fuzzy warning messages when applying a patch can be ignored
    * **Autoconf Build System**
        - accepts system types in a triplet, cpu-vendor-kernel-os, where vendor field can be
          omitted, e.g. x86_64-redhat-linux
        - two systems can share the same kernel and be different to use the same triplet to
          describe them, e.g. Android and Ubuntu on ARM64 using the same Linux kernel
        - can check system triplet with ``config.guess`` script from source files, or
          ``gcc -dumpmachine``
        - can check the dynamic linker with ``readelf -l any_binary | grep interpreter``
    * install the cross-toolchain under ``$LFS/tools`` be keep them separate

Directories
-----------
    * enter a chroot environment to install the final LFS system
    * **Virtual Kernel File Systems**
        - created by the kernel, and used by applications to communicated with it
        - virtual file systems, and no disk space is used as contents are in memory
        - mount the necessary in LFS directory tree, such as ``dev``, ``proc``, ``sys``, ``run``
        - kernel auto mounts ``devtmpfs`` on ``/dev`` and create device device nodes
        - some hosts can be without ``devtmpfs`` support, and need to bind mount the host's
          ``/dev`` with ``mount -v --bind /dev $LFS/dev``
        - mount others with ``mount -vt devpts devpts -o gid=5,mode=0620 $LFS/dev/pts``,
          ``mount -vt proc proc $LFS/proc``, ``mount -vt sysfs sysfs $LFS/sys``,
          ``mount -vt tmpfs tmpfs $LFS/run``
        - may need to explicitly mount a tmpfs with
          ``mount -vt tmpfs -o nosuid,nodev tmpfs $LFS/dev/shm``
        - can check VFS mounts with ``findmnt | grep $LFS``
    * **Entering Chroot**

        .. code-block:: bash

           chroot "$LFS" /usr/bin/env -i \
           HOME=/root \
           TERM="$TERM" \
           PS1='(lfs chroot) \u:\w\$ ' \
           PATH=/usr/bin:/usr/sbin \
           MAKEFLAGS="-j$(nproc)" \
           TESTSUITEFLAGS="-j$(nproc)" \
           /bin/bash --login


    * **Full Directory Structure**
        - create root-level directories with ``mkdir -pv /{boot,home,mnt,opt,srv}``
        - create subdirectories ``/etc/{opt,sysconfig}``, ``/lib/firmware``,
          ``/media/{floppy,cdrom}``, ``/usr/{,local/}{include,src}``, ``/usr/lib/locale``,
          ``/usr/local/{bin,lib,sbin}``, ``/usr/{,local/}share/{color,dict,doc,info,locale,man}``,
          ``/usr/{,local/}share/{misc,terminfo,zoneinfo}``, ``/usr/{,local/}share/man/man{1..8}``,
          ``/var/{cache,local,log,mail,opt,spool}``, ``/var/lib/{color,misc,locate}``
        - create symlinks with ``ln -sfv /run /var/run``, ``ln -sfv /run/lock /var/lock``,
          ``ln -sv /proc/self/mounts /etc/mtab``
        - set attributes with ``install -dv -m 0750 /root``, ``install -dv -m 1777 /tmp /var/tmp``
        - create log files with ``touch /var/log/{btmp,lastlog,faillog,wtmp}``,
          ``chgrp -v utmp /var/log/lastlog``, ``chmod -v 664 /var/log/lastlog``,
          ``chmod -v 600 /var/log/btmp``
    * **/etc/hosts File**


        127.0.0.1 localhost $(hostname)
        ::1       localhost


    * **/etc/passwd File**


        root:x:0:0:root:/root:/bin/bash
        bin:x:1:1:bin:/dev/null:/usr/bin/false
        daemon:x:6:6:Daemon User:/dev/null:/usr/bin/false
        messagebus:x:18:18:D-Bus Message Daemon User:/run/dbus:/usr/bin/false
        uuidd:x:80:80:UUID Generation Daemon User:/dev/null:/usr/bin/false
        nobody:x:65534:65534:Unprivileged User:/dev/null:/usr/bin/false


    * **/etc/group File**
        - LSB only recommends GID 0 ``root`` and GID 1 ``bin``
        - GID 5 is widely used for ``tty`` group, and other names and GID can be chosen freely
        - well-written programs do not depend on GID numbers, but the name


        root:x:0:
        bin:x:1:daemon
        sys:x:2:
        kmem:x:3:
        tape:x:4:
        tty:x:5:
        daemon:x:6:
        floppy:x:7:
        disk:x:8:
        lp:x:9:
        dialout:x:10:
        audio:x:11:
        video:x:12:
        utmp:x:13:
        cdrom:x:15:
        adm:x:16:
        messagebus:x:18:
        input:x:24:
        mail:x:34:
        kvm:x:61:
        uuidd:x:80:
        wheel:x:97:
        users:x:999:
        nogroup:x:65534:


    * set locale with ``localedef -i C -f UTF-8 C.UTF-8``
    * can add a temporary user to run tests with
      ``echo "tester:x:101:101::/home/tester:/bin/bash" >> /etc/passwd``,
      ``echo "tester:x:101:" >> /etc/group``, and ``install -o tester -d /home/tester``

Backup and Restore
------------------
    * after creating essential programs and libraries, LFS system should be backed up in case
      of failures in the future
    * create a backup outside the chroot environment
    * umount VFS with ``mountpoint -q $LFS/dev/shm && umount $LFS/dev/shm``,
      ``umount $LFS/dev/pts``, ``umount $LFS/{sys,proc,run,dev}``
    * change into LFS directory, and create a backup archive with
      ``tar -cJpf $HOME/lfs-temp-tools.12.2.tar.xz .``
    * restore with ``rm -rf $LFS/*``, and ``tar -xvpf $HOME/lfs-temp-tools.12.2.tar.xz -C $LFS/``

`back to top <#linux>`_

References & External Resources
===============================

* Corbet, J., Rubini A., Kroah-Hartman, G. (2005). Linux Device Drivers, Third Edition.
  Sebastopol, CA: O'Reilly Media, Inc.
* Kroah-Hartman, G. (2007). Linux Kernel in a Nutshell. Sebastopol, CA: O'Reilly Media, Inc.
* Beekmans, Gerard. (2024). Linux From Scratch [online]. Available at:
  https://www.linuxfromscratch.org/lfs/
* The Linux Foundation. (2024). LF Live: Mentorship Series. Available at:
  https://events.linuxfoundation.org/lf-live-mentorship-series/
* The Linux Foundation. (2024). LFX Mentorship. Available at:
  https://lfx.linuxfoundation.org/tools/mentorship/
* The Linux Foundation. (2017). Introduction to Memory Management in Linux - Matt Porter,
  Konsulko. Available at: https://youtu.be/7aONIVSXiJ8?si=4lVDrJKYy9vn6QNM
* The Linux Foundation. (2017). Getting Into Linux Kernel Development After 30 Years - Muhammad
  Usama Anjum, Collabora. Available at: https://youtu.be/xtXn45cnVzE?si=w9TIYBbfNBiWlqwL

`back to top <#linux>`_
