---
title:  "Linux Boot Process"
subtitle: "It's always a bit messy"
author: "Shamsher Kushwaha"
avatar: "img/authors/43068991.png"
image: "img/apache-security-hardening-guide.png"
date:   2020-11-29 20:51:12
---
### Linux Boot Process:-
	There are the 6 high level stages in Linux boot process.
	1 - BIOS - BIOS stands for Basic Input/Output System
	2 - MBR - MBR stands for Master Boot Record.
	3 - GRUB - GRUB stands for Grand Unified Bootloader
	4 - Kernel - 
	5 - init
	6 - Runlevel
		0	Halt
		1	Single-User Mode
		2	Multi-User Mode
		3	Multi-User Mode with Networking
		4	Undefined
		5	display manager GUI
		6	Reboot



	1. BIOS = (Basic Input/Output System)
		It looks for boot loader in floppy, cd-rom, or hard drive.
		Once the boot loader program is detected and loaded into the memory,
		So, in simple terms BIOS Search, loads and executes the MBR Master boot loader Program.
		Once the Master boot loader Program detceted and loaded into memory, BIOS Gives Control to it.
		
		
	2. MBR = (Master Boot Record)
		It is located in the 1st sector of the bootable disk.
		MBR is less than 512 bytes in size.
		This has three components 1) primary boot loader info in 1st 446 bytes 
								2) partition table info in next 64 bytes 
								3) mbr validation check in last 2 bytes.	
		It contains information about GRUB (or LILO in old systems).
		So, in simple terms MBR loads and executes the GRUB boot loader.
		
	3. GRUB = (Grand Unified Bootloader)
		Grub configuration file is /boot/grub/grub.conf
		So, in simple terms GRUB just loads and executes Kernel and initrd images.
		
	4. Kernel = 
		Mounts the root file system as specified in the grub.conf
		Kernel executes the /sbin/init program . Initd is used by kernel as temporary root file system until kernel mounts the root files system. 
		
	5. Init
		Looks at the /etc/inittab file to decide the Linux run level.
		Run levels decide which initil programs are loaded at startup
		Following are the available run levels. 0 – halt, 1 – Single user mode, 2 – Multiuser, without NFS, 3 -CLI,  4 – unused ,5 - Display, nitit 6

	6. Runlevel programs
		Depending on your init level setting, the system will execute the programs from one of the following directory.
		Runlevel0=/etc/rc.d/rc.0d/
		Runlevel1=/etc/rc.d/rc.0d/
		"
		Runlevel6=/etc/rc.d/rc.6d/
		
	
### Please describe the Linux Process.

	There are seven steps to the boot-up sequence. 
	1) BIOS (basic input/output system) – executes the MBR where
	Boot Loader sits, 
	2) MBR- Master boot reads Kernel into memory,
	3) GRUB (Grand Unified Bootloader) Kernel
	starts Init process, 
	4) Kernel – Kernel executes the /sbin/init program. Init reads inittab, executes rc.sysinit, 
	5) Init
	– the rc script than starts services to reach the default run level and 6) Run level programs – these programs are
	executed from /etc/rc.d/rc*.dl/

	Q What are the Linux boot files?
	1./boot/grub/grub.conf: contains boot disk parameters
	2./etc/fstab: contains File systems which need to mount at boot time
	3./etc/initab: Contains default run level
	4./etc/init.d/rc.d/rcN.d: This is a dir it contains

	Q = /etc/fstab Details
	The first field=device name , The second field=mount point, Third field=filesystem type, The fourth field=describes the mount options., 
	Fifth field contains a 1 if the dump utility should back up a partition or a 0 if it shouldn’t.
	Sixth field specifies the order in which fsck checks the device/partition for errors at boot time.A 0 means that fsck should not check a file system.

	Q = Understanding /etc/passwd file fields
	Ans = Username : Passwd X (/etc/shadow file) : User ID : Group ID : User Info : Home directory  : Command/shell

	Q = Reset Root Password in CentOS
	Ans = 1). Restart the system, then tap the Esc key about once per second to launch the GRUB menu.
		2). Use the arrows to highlight the version of Linux you boot into, then press e.
		3). Use the arrows to highlight the line that starts with kernel or Linux. Press E.
		4). At the end of the line Linux16 , type init=/bin/bash OR (rd.break)  . Once done editing press 'ctrl+x'
		5). mount -o remount,rw /sysroot
		6). chroot /sysroot
		6). passwd root
		7). touch /.autorelabel      		(Set SELinux relabeling on next boot)
		8). exit
		9). exit  => reboot
	Q= How to Recover or Rescue Corrupted Grub Boot Loader in CentOS 7
	Ans = 1). centos 7 bootable USB stick. Place the bootable image into your machine appropriate drive and reboot the machine.
		2). select your cd-ROM or USB Drive => secect Troublishooting Option  => Rescue a CentOS system 
		3). Then press Enter => After the installer software loads into your machine RAM, the rescue environment prompt will appear on your screen. 
									On this prompt type 1 in order to Continue with the system recovery process, 
		4). chroot /mnt/sysimage
		5). /sbin/grub2-install /dev/sda
		6). Exit  => reboot   => After machine restart, you should, first, enter BIOS settings and change the boot order menu 
	  
	  

## RHEL 6 vs RHEL 7 Difference

	Feature Name				RHEL 6								RHEL 7
	Default File System			Ext4								XFS
	Kernel Version				2.6.xx								3.10.xx
	Release Name				Santiago							Maipo
	Gnome Version				GNOME 2								GNOME 3.8
	KDE Version					KDE 4.1								KDE 4.6
	Samba Version				SMB 3.6								SMB 4.4
	Default Database			MySQL								MariaDB
	Boot Loader					Grub 0.97=/boot/grub/grub.cfg		Grub 2 = /boot/grub2/grub.conf
	Process ID					Initd  Process ID 1					Systemd Process ID 1
	Boot Time					40 Sec								20 Sec
	File System Check			e2fsck								xfs_repair
	Resize filesystem			resize2fs 							xfs_growfs
	File System Size			EXT4 16TB with XFS 100TB			XFS 500TB with EXT4 16TB
	Processor Architecture		32Bit and 64Bit						Only 64Bit.
	Host name Config File		/etc/sysconfig/network				/etc/hostname 
	Interface Name				eth0								ens33xxx
	Managing Services			service sshd start					systemctl start sshd.service
	System Logs					/var/log/							/var/log journalctl
	Run Levels					7 runlevel							no run levels, Run levels are called as targets
	UID Information				500 to 65534						1000 – 65534
	By Pass Root Password Prompt append 1 or s or 					Append rd.break



	Linux lvm - Logical Volume Manager
	Logical Volume Management (LVM) makes it easier to manage disk space.
	##Partition the new disk space
	fdisk /dev/sda
	Command (m for help): n
	Command action p 
	Partition number (1-4): 3
	Command (m for help): t
	Hex code (type L to list codes): 8e
	Command (m for help): w

pvcreate /dev/sdb /dev/sdc /dev/sdd
vgcreate vg_name /dev/sdb /dev/sdc
lvcreate -L 400 -n Newlv_name vg_name      (400 MB -L 400, 4 GB -L 4G)
mkfs.ext3 -m 0 /dev/mynew_vg/vol01
Edit /etc/fstab
Mount logical volumes
# mkdir /home/foobar 
#mount -a

## Extend logical volume

	#lvextend -L +5G /dev/centos/var
	resize2fs /dev/centos/var    (for ext4)
	xfs_growfs /dev/centos/var   (for xfs)
	#resize2fs /dev/mapper/vg_cloud-LogVol00


## Reduce logical volume

	#umount /home/
	#e2fsck -f /dev/mapper/vg_cloud-LogVol00       (check the file system for Errors using e2fsck command.)
	#lvreduce -L -8G /dev/vg_tecmint_extra/tecmint_reduce_test
	#e2fsck -f /dev/mapper/vg_cloud-LogVol00      (For the safer side, now check the reduced file system for errors)
	#resize2fs /dev/vg_tecmint_extra/tecmint_reduce_test
	#mount /dev/vg_tecmint_extra/

	#df -hT , lsblk -f, cat /etc/fstab, 
	#cat /etc/mtab


### What is File System:- It is responsible for storing information on disk and retrieving and updating this information.

	Diffrent Between Ext4 and XFS
	Ext2
		Ext2 does not have journaling feature.
		On flash drives, usb drives, ext2 is recommended, as it doesn’t need to do the over head of journaling.
		Maximum individual file size can be from 16 GB to 2 TB
		Overall ext2 file system size can be from 2 TB to 32 TB
	Ext3
		Starting from Linux Kernel 2.4.15 ext3 was available.
		The main benefit of ext3 is that it allows journaling.
		Journaling has a dedicated area in the file system, where all the changes are tracked. When the system crashes, the possibility of file system corruption is less because of journaling.
		Maximum individual file size can be from 16 GB to 2 TB
		Overall ext3 file system size can be from 2 TB to 32 TB
		# mkfs.ext3 /dev/sda1
	Ext4
		Ext4 stands for fourth extended file system.It was introduced in 2008.Starting from Linux Kernel 2.6.19 ext4 was available.
		Maximum individual file size can be from 16 GB to 16 TB. Overall maximum ext4 file system size is 1 EB (exabyte). 1 EB = 1024 PB 
		In ext4, you also have the option of turning the journaling feature “off”.
		File system check = e2fsck, Resizing a file system = resize2fs, Backup a file system = dump and restore, Create a file system = mkfs.ext4
		# mkfs.ext4 /dev/sda1
	XFS
		The XFS is a high-performance 64-bit journaling file system.The support of the XFS was merged into Linux kernel in around 2002 .
		XFS supports maximum file system size of 8 exbibytes for the 64-bit file system. Now, the RHEL 7.0 uses XFS as the default filesystem with 3.6 kernel.
		File system check = xfs_repair, Resizing a file system = xfs_growfs, Backup a file system = xfsdump and xfsrestore 
		Create File System = mkfs.xfs,
		##mkfs.xfs -f /dev/sdb1 

## Some QA

	Q:- What is filesystem.
	Ans:- Linux File System or any file system generally is a layer which is under the operating system that handles the positioning of your data on the storage, without it; the system cannot knows which file starts from where and ends where.

	JFS: old file system made by IBM. It works very well with small and big files, but it failed and files corrupted after long time use, reports say.
	===================================================================================================================================================================
	/ :	  This is the root directory which should contain only the directories needed at the top level of the file structure

	/bin: Essential User Command Binaries

	/boot: Where boot loader and boot files are located.

	/dev: Where all physical drives are mounted like USBs DVDs.

	/etc: Contains configurations for the installed packages.

	/home: User Home Directory

	/lib: Where the libraries of the installed packages located since libraries shared among all packages

	/media: Removable Media

	/mnt: Where you mount other things Network locations and some distros you may find your mounted USB or DVD.

	/opt: Some optional packages are located here and this is managed by the package manager.

	/proc: Because everything on Linux is a file, this folder for processes running on the system,

	and you can access them and see much info about the current processes.

	/root: The home folder for the root user.

	/sbin: System Binary

	/tmp: Contains the temporary files.

	/usr: User Utility and Application

	/var: Contains system logs and other variable data.

	================================================================================================================================================================
	Linux Process
	All You Need To Know About Processes in Linux 
	Q = Process = A process refers to a program in execution; it’s a running instance of a program. It is made up of the program instruction, 
				data read from files, other programs or input from a system user.

	Types of Processes = There are fundamentally two types of processes in Linux:
	Foreground processes (also referred to as interactive processes) – these are initialized and controlled through a terminal session. In other words,
				there has to be a user connected to the system to start such processes; they haven’t started automatically as part of the system functions/services.
				
	Background processes (also referred to as non-interactive/automatic processes) – are processes not connected to a terminal; they don’t expect any user input.

	Q = How Does Linux Identify Processes?
	Because Linux is a multi-user system, meaning different users can be running various programs on the system, each running instance of a program must be identified uniquely by the kernel.

	And a program is identified by its process ID (PID) as well as it’s parent processes ID (PPID), therefore processes can further be categorized into:
	Parent processes – these are processes that create other processes during run-time.
	Child processes – these processes are created by other processes during run-time.

	Q = The Init Process
	Init process is the mother (parent) of all processes on the system, it’s the first program that is executed when the Linux system boots up;
	it manages all other processes on the system. It is started by the kernel itself, so in principle it does not have a parent process.
	The init process always has process ID of 1. It functions as an adoptive parent for all orphaned processes.

### You can use the pidof command to find the ID of a process:

	pidof systemd
	pidof top
	pidof httpd

### To find the process ID and parent process ID of the current shell, run:

	$ echo $$
	$ echo $PPID

### glances – System Monitoring Tool   (Just Like Top Command)
	# glances
	# Lsof – List Open Files
	# Tcpdump – Network Packet Analyzer
	# Netstat – Network Statistics
	# Htop – Linux Process Monitoring
	# Iotop – Monitor Linux Disk I/O
	# Iostat – Input/Output Statistics
	# IPTraf – Real Time IP LAN Monitoring
	# Psacct or Acct – Monitor User Activity
	# Monit – Linux Process and Services Monitoring (Monit is a free open source and web based process supervision utility that automatically monitors)
	It monitors services like Apache, MySQL, Mail, FTP, ProFTP, Nginx, SSH and so on. The system status can be viewed from the command line or using it own web interface.
	# NetHogs – Monitor Per Process Network Bandwidth
	# iftop – Network Bandwidth Monitoring
	# Arpwatch – Ethernet Activity Monitor

### Process states in Linux

	Different process states now.
	1. Running or Runnable = 
	2. Sleeping or waiting = 
	3. Stopped = 
	4. Zombie =  


### Linux Security & Hardeing - A complete Course Module

	1-> Physical Security of linux 
		Set Bios Passwd
		Singal User Mode Security
		Secure Boot Loader
	2-> PAM (Pluggable Authentication Modules)
	3-> Account Security
	4-> File Security
	5-> Network Security
	
## QA

	1) What is Linux?
	Linux is an operating system based on UNIX and was first introduced by Linus Torvalds. It is based on the Linux Kernel and can run on 
	different hardware platforms manufactured by Intel, MIPS, HP, IBM, SPARC, and Motorola.(1991)

	2) What is the difference between UNIX and LINUX?
	Unix originally began as a propriety operating system from Bell Laboratories, which later on spawned into different commercial versions.
	On the other hand, Linux is free, open source and intended as a non-propriety operating system for the masses.

	3) What is BASH?
	BASH is short for Bourne Again SHell. It was written by Steve Bourne as a replacement to the original Bourne Shell (represented by /bin/sh). 
	It combines all the features from the original version of Bourne Shell, plus additional functions to make it easier and more convenient to use.
	It has since been adapted as the default shell for most systems running Linux.
	
	4) What is Linux Kernel?
	The Linux Kernel is a low-level systems software whose main role is to manage hardware resources for the user. 
	It is also used to provide an interface for user-level interaction.

	5)What is a shell? What are their names?
	The shell is the part of the system with which the user interacts. A Unix shell interprets commands such as “pwd”,

	5) What is LILO?
	LILO is a boot loader for Linux. It is used mainly to load the Linux operating system into main memory so that it can begin its operations.

	6) What is ilo?
	you access the server remotely, start an iLO remote console session., Power on and power off the server., Restart the server., Monitor the server, 

	6) What is a swap space?
	Swap space is a certain amount of space used by Linux to temporarily hold some programs that are running concurrently. 
	This happens when RAM does not have enough memory to hold all programs that are executing.

	6) How To Create a Linux Swap File.
	When the kernel runs out of memory, it can move idle/inactive processes into swap creating room for active processes in the working memory. 
	This is memory management that involves swapping sections of memory to and from virtual memory.
	Example = In this example, we will create a swap file of size 2GB using the dd command as follows.
	# dd if=/dev/zero of=/mnt/swapfile bs=1024 count=2097152 (O)R USE THIS (sudo fallocate -l 1G /swapfile) ,>>  # fallocate --length 2GiB /mnt/swapfile 
	>> #chmod 600 /mnt/swapfile >>
	>># mkswap /mnt/swapfile  >>#swapon /mnt/swapfile   >>#Edit the /etc/fstab 
	/mnt/swapfile swap swap defaults 0 0      (/mnt/swapfile – device/file name , swap – defines device mount point , swap – specifies the file-system type , )
												(defaults – describes the mount options, 0 – specifies the option to be used by the dump program , 0 – specifies the fsck command option)
	-
	To set how often the swap file can be used by the kernel, open the /etc/sysctl.conf 
	While the swappiness value of 60 is OK for Desktops, you can put 100 also 
	vm.swappiness=10
	# swapon  -s
	# free

	7)What is a buffer cache?
	The buffer cache is main memory used as a cache to reduce the number of physical read/writes from mass-storage devices.
	The buffer cache is just one of several types of cache used to reduce to a minimum the number of slow operations.


	26. What is YUM?
	YUM stands for Yellow dog Updater,
	YUM, were written by the Linux community as a way to maintain an RPM-based system


	10) What is the basic difference between BASH and DOS?
	- Under BASH, / character is a directory separator and \ acts as an escape character. 
	Under DOS, / serves as a command argument delimiter and \ is the directory separator

	12) Describe the root account.
	The root account is like a systems administrator account and allows you full control of the system. 
	Here you can create and maintain user accounts, assigning different permissions for each account. Symbol #

	11) what is UMASK
	umask is a command that determines the settings of a mask that controls how file permissions are set for newly created files.
	vi ~/.bashrc


	12) What is INODE.
	It is a structure which has the description of all the files and pointers to the data blocks of files stored in it.
	The information contained is file-size, access and modification time, permission and so on.
	Each file is given a unique name by the operating system which is called as the inode.

	13) What is kdump in Linux.
	Ans:- kdump is a feature of the Linux kernel that creates crash dumps in the event of a kernel crash.
		Kdump is an utility used to capture the system core dump in the event of system crashes.

	16) How can you find out how much memory Linux is using?
	cat /proc/meminfo
	free - m
	vmstat
	top
	htop

	18) What are symbolic links?
	It also allows you instant access to it without having to go directly to the entire pathname.


	Q Explain briefly the procedure for re-installing Grub in Linux?
	Ans =Select Local CD/DVD
		Mount your Linux root partition = sudo mount /dev/sda6 /mnt ( Assuming /dev/sda6 is the Linux root partition)
		Install / reinstall grub = sudo grub-install --root-directory=/mnt/ /dev/sda ( where /dev/sda is your primary disk)
		6) Reboot your system, remove bootable CD
		
	Q How do you boot your system into the following modes, when you are in some trouble?
	Ans= a) Rescue mode = Rescue mode provides the ability to boot a small Linux environment from an external bootable device like a CD-ROM, or USB
		b) Single user mode = In single-user mode, the system boots to runlevel 1, press any key to enter the GRUB interactive menu., type "a"
			Go to the end of the line and type "single" as a separate word.Press Enter to exit edit mode and type "b" to boot into single usermode now.
			
		c) Emergency mode = In emergency mode,The root file system is mounted read-only.

	Q:30 What is load average in Linux ?
	Ans = Load Average is defined as the average sum of the number of process waiting in the run queue and number of
	process currently executing over the period of 1,5 and 15 minutes(exam = " 0.12 1.65 2.3 "). Using the „top‟ and „uptime‟ command we find
	the load average of a linux sever

	20) How do you refer to the parallel port where devices such as printers are connected?
	under Linux you refer to it as /dev/lp . LPT1, LPT2 and LPT3 would therefore be referred to as /dev/lp0, /dev/lp1, or /dev/lp2 under Linux.

	21) Are drives such as hard drive and floppy drives represented with drive letters?
	floppy drives are referred to as /dev/fd0 and /dev/fd1. IDE/EIDE hard drives are referred to as /dev/hda, /dev/hdb, /dev/hdc, and so forth.


	23) In Linux, what names are assigned to the different serial ports?
	Serial ports are identified as /dev/ttyS0 to /dev/ttyS7.

	24)Difference Between Hard link and Soft link
	-SoftLink-A file can be accessed through different references pointing to that file is known as a soft link.
	when the original file is deleted file will loss.  only softlink can set on directory not hardlink.
	Its inode number is different. Command = ln -s,.Can be linked To any other file system even networked.Relative Path Allowed.

	-HardLink-A file can be accessed through many different names known as hard links.when the original file is deleted file will 
	Still valid and file can be accessed. "command = ln" .Its inode number	is Same. Can be linked	To its own partition.
	The performance of hard link is better than soft link in some cases. (1=nohard link , 2=hardlink now)

	26) What is the maximum length for a filename under Linux?
	Any filename can have a maximum of 255 characters.

	27)What are filenames that are preceded by a dot?
	In general, filenames that are preceded by a dot are hidden files.

	32) What are daemons?
	Daemons are services that provide several functions that may not be available under the base operating system.

	36) What are environmental variables?
	Environmental variables are global settings that control the shell's function as well as that of other Linux programs.

	37) What are the different modes when using vi editor?
	There are 3 modes under vi:- Command mode – this is the mode where you start in-
	Edit mode – this is the mode that allows you to do text editing-
	Ex mode – this is the mode wherein you interact with vi with instructions to process a file

	39) What is redirection?
	Redirection is the process of directing data from one output to another.
	Input Redirection: ‘<’ symbol is used for input redirection and is numbered as (0).
	Output Redirection: ‘>’ symbol is used for output redirection and is numbered as (1).
	Error Redirection: It is denoted as STDERR(2).

	42) What are the contents of /usr/local?
	It contains locally installed files.

	47) Write a command that will look for files with an extension "c", and has the occurrence of the string "apple" in it.
	Find ./ -name "*.c" | xargs grep –i "apple"

	48) Write a command that will display all .txt files, including its individual permission.
	ls -al *.txt

	52) How can you find the status of a process?
	ps ux

	52) What is the difference between su and sudo?
	sudo executes commands while the environment of current user loaded. With su you can load complete environment of destination account.

	60) Explain how to enable root logging in Ubuntu?
	sudo sh-c 'echo "greater-show-manual-login=true" >>/etc/lightdm/lightdm.conf'

	61) How can you run a Linux program in the background simultaneously when you start your Linux Server?
	By using nohup. It will stop the process receiving the NOHUP signal and thus terminating it 
	you log out of the program which was invoked with. & runs the process in the background.

	Que 3) Enlist the basic components of LINUX?
	Ans: Linux operating system basically consists of 3 components which are enlisted below
	Kernel: This is considered as the core part and is responsible for all major activities of Linux operating system.
	Linux Kernel is considered as free and open source software which is capable of managing hardware resources for the users.
	It consists of various modules and interacts directly with the underlying hardware.

	System Library: Most of the functionalities of the operating system are implemented by System Libraries.
	These act as a special function using which application programs accesses Kernel’s features.

	System Utility: These programs are responsible for performing specialized, individual level tasks.

	Que 20) Differentiate between Cron and Anacron?
	Cron allows the user to schedule tasks to be executed every minute.Anacron allows the user to schedule tasks to be run either on a specific date

	Que 27) Enlist some Linux file content commands?
	head: Displays the beginning of the file
	tail: Displays the last part of the file
	cat: Concatenate files and print on the standard output.
	more: Displays the content in pager form and is used to view text in the terminal
	less: Displays the content in pager form and allows backward and single line movement.

	Que 32) Explain the Linux ‘cd’ command options along with the description?
	cd~: Brings you to the home directory
	cd-: Brings you to the previous directory
	. : Bring you to the parent directory
	cd/: Takes you to the entire system’s root directory

	Que 35) Enlist some Linux networking and troubleshooting commands?
	netstat: It displays network connections, routing tables, interface statistics.
	Traceroute: It is network troubleshooting utility
	Dig: This command is used to query the DNS name servers
	nslookup: To find DNS related query.
	Ifplugstatus: This command tells us whether the network cable is plugged in or not.

	Q #4) What is called Shell?
	The interface between user and system called a shell. Shell accepts commands and set them to execute for user operations.

	Q #31) What is the command to find remaining disk space in UNIX server?
	df -kl

	Q #35) Discuss the difference between swapping and paging?
	Swapping – Complete process is moved to main memory for execution.
	Paging – Only the required memory pages are moved to the main memory for execution.

	Question 10. What Is A Zombie?
	Answer :Zombie is a process state when the child dies before the parent process.

	Question 11. Explain Each System Calls Used For Process Management In Linux.
	Answer :
	System calls used for Process management:
	Fork () :- Used to create a new process
	Exec() :- Execute a new program
	Wait():- wait until the process finishes execution
	Exit():- Exit from the process
	Getpid():- get the unique process id of the process
	Getppid():- get the parent process unique id
	Nice():- to bias the existing property of process

	Question 22. What Is A Filesystem?
	Answer :Sum of all directories called file system.
	A file system is the primary means of file storage in UNIX. File systems are made of inodes and superblocks.

	Question 36. What Is The Difference Between Home Directory And Working Directory?
	Answer :
	Home directory is the directory you begin at when you log into the system. 
	Working directory can be anywhere on the system and it is where you are currently working.   

	Question 47. How Can You See All Mounted Drives?
	Answer :mount -l

	Question 47. How Can You See All unMounted Drives?
	Answer :cat /etc/mtab

	Question 55. How To Find Difference In Two Configuration Files On The Same Server?
	diff -u /usr/home/my_project1/etc/ABC.conf /usr/home/my_project2/etc/ABC.conf

	Question 56. What Is The Best Way To See The End Of A Logfile.log File?
	tail -n file_name ( the last N lines, instead of the last 10 as default)

	Question 67. Explain Mysql Architecture.
	Answer :The front layer takes care of network connections and security authentications, the middle layer does the SQL query parsing, 
	and then the query is handled off to the storage engine.

	Question 71. What Is Stack?
	Stack is a portion of RAM used for saving the content of Program Counter and general purpose registers.

	Question 74. What Is A Compiler?
	Compiler is used to translate the high-level language program into machine code at a time.
	It doesn’t require special instruction to store in a memory, it stores automatically.

	Question 75. Differentiate Between Ram And Rom?
	RAM: Read / Write memory, High Speed, Volatile Memory. ROM: Read only memory, 

	Question 78. What Is Cache Memory?
	Cache memory is a small high-speed memory. It is used for temporary storage of data & information between the main memory and the CPU.
	The cache memory is only in RAM.

	Question 81. What Is The Difference Between Primary & Secondary Storage Device?
	In primary storage device the storage capacity is limited.secondary storage device the storage capacity is larger.
	Primary devices are: RAM / ROM. Secondary devices are: Floppy disc I Hard disk.

	Question 132. What Command Should You Use To Check Your File System?
	Ans : - fsck

	Question 139. What Command Can You Use To Review Boot Messages?
	Ans : - dmesg

	10. How to check and verify the status of the bond interface?
	Ans: - cat /proc/net/bonding/bond0

	25. Is there any relation between modprobe.conf file and network devices?
	Yes, this file assigns a kernel module to each network device.# cat /etc/modprobe.conf

	26 What is the role of Kudzu?
	Kudzu is used to detect new Hardware. RedHat Linux runs a hardware discoverer, named kudzu.

	31. How to Enable ACLs for /home partition?
	Add following entry in /etc/fstab
	LABEL=/home /home ext3 acl 1 2
	Now remount /home partition with acl option.
	mount -t ext3 -o acl /dev/sda3 /home

	33) How to Mount a windows ntfs partition on Linux?
	mount sudo mount ­t ntfs­3g /dev/<Windows­partition>/<Mount­point>”

	Q:7 Where the kernel modules are located ?
	Ans: The ‘/lib/modules/kernel-version/’ directory stores all kernel modules

	Q:8 What is umask ?
	Ans: umask stands for ‘User file creation mask’, which determines the settings of a mask that controls which file 
	permissions are set for files and directories when they are created.

	Q:11 How to share a directory using nfs ?
	/etc/exportfs
	‘/<directory-name>  <ip or Network>(Options)’ 
	/home/demo 			*(rw)
	/vol1                192.168.2.21(ro,sync)

	DHCP         67/UDP(dhcp server) , 68/UDP(dhcp client)
	Squid         3128
	NTP				123 

	Q:18 How to check which ports are listening in my Linux Server ?
	‘netstat –listen’ and ‘lsof -i’

	Q:19 List the services that are enabled at a particular run level in linux server ?
	‘chkconfig –list | grep 5:on’
	
	Q:21 How to upgrade Kernel in Linux ?
	Ans: We should never upgrade Linux Kernel , always install the new New kernel using rpm command 
	because upgrading a kenel can make your linux box in a unbootable state.

	1) Which files stores the user min UID, max UID, password expiration settings, password encryption method being used etc.,?
	ANS : /etc/login.defs

	2) How do you make a file copied to a new user account automatically upon user account creation?
	ANS : Store the file in /etc/skel directory.

	8) How do you make a new user to reset his password upon his first login?
	# chage -d 0 mango

	9) Create users home directory in /home1 directory instead of default 
	ANS: – Edit /etc/default/useradd

	11) How to check if an user account has been locked?
	# passwd -S username

	2) How to check if the syslog service is running?
	systemctl status rsyslog.service

	3) By default log files are set to get rotated on weekly basis, how to make this gets rotated on monthly basis?
	Edit /etc/logrotate.conf

	5) How to increase size of ‘kernel ring buffer’ file (dmesg)?
	ANS:- By default the kernel ring buffer size is 512 bytes. So, 
	to increase this space add “log_buf_len=4M” to the kernel stanza in grub.conf file.

	6) What does /var/log/wtmp and /var/log/btmp files indicates and what do they store?
	ANS:- These files are used to store user login/logout details since from the date of creation.

	5) How do you find out all the packages installed on a RHEL system(server)?
	rpm -qa

	8)How to view the installed date of a package (consider the package sg3_utils)?
	ANS:- Check in /var/log/yum.log file (provided the package is installed by yum):-

	7) How to verify if a filesystem state is marked as clean?
	# dumpe2fs -h /dev/sda1 |grep -i state

	11) Different fields in /etc/fstab.
	ANS : – DeviceName MountPoint FilesystemType MountOptions DumpFrequency FsckCheckOrder

	1) I’ve installed the latest kernel on the system successfully, however, my server still boots from the old kernel. 
	How do you make the system to boot from the newly installed kernel?
	ANS : – Verify if the new kernel packages are installed successfully.
	– Verify if the kernel stanza is added in grub.conf file.
	– Make the new kernel as the default kernel to boot in grub.conf file.

	3) Which command is to be used to create GRUB password to avoid normal user in editing GRUB interface while booting?
	ANS :- Using the command “grub-crypt” (in RHEL 6.x). This generates an encrypted password using “sha512” algorithm.

	1) How do you change the network speed of an interface to 100Mbps with auto-negotiation off and duplex in full mode(example for interface eth0)?
	#ethtool -s eth0 speed 100 autoneg off duplex full


	Q You have a large text file, and you need to see one page at a time. What will you do?
	Ans = # cat file_name.txt | more

	Q Who own the data dictionary?
	Ans = The user 'SYS' owns the data dictionary. Users 'SYS' and 'SYSEM are created by default, automatically

	Q What command should you use to check the number of files and disk space used by each
	user's defined quotas?
	Ans = 'repquota

	Q How will you restrict IP so that the restricted IP‟s may not use the FTP Server?
	Ans = We can block suspicious IP by integrating tcp_wrapper. We need to enable the parameter “tcp_wrapper=YES” in the
	configuration file at „/etc/vsftpd.conf‟. And then add the suspicious IP in the „host.deny‟ file at location „/etc/host.deny‟.

	Q What is RAID?
	Ans = Redundant Array of Inexpensive Disks. RAID is a method by which same data or information is
	spread across several disks, using techniques such as disk striping (RAID Level 0),(RAID Level 1),(RAID Level 5),(RAID Level 10)

	Q What is the status code 403,404 represented in apache server?
	403 represent forbidden error, means if a file misses some selinux security context.
	404 represent that there is a cgi script missing or web pages missing.
	500 

	Q Is there any relation between modprobe.conf file and network devices?
	Ans = Yes, this file assigns a kernel module to each network device.
	]# cat /etc/modprobe.conf


	Q What is the difference between soft and hard mounting points ?
	Ans = In the soft mount, if the client fails to connect the server, it gives an error report and closes the connection whereas in hard mount, 
	if the client fails to access the server, the connection hangs; and once the system is up, it again accesses the server.

	Q What is df -i command ?
	Ans df -i

	Q What is initrd image and what is its function in the linux booting process?
	The initial RAM disk (initrd) is an initial root file system that is mounted prior to when the real root file system is available.
	The initrd is bound to the kernel and loaded as part of the kernel boot procedure. 
	The kernel then mounts this initrd as part of the two-stage boot process to load the modules to make the real file systems available 
	and get at the real root file system. Thus initrd image plays a vital role in linux booting process.

	Q What is the difference between umask and ulimit ?
	umask stands for ‘User file creation mask’, which determines the settings of a mask that controls which file permissions are set for files and directories when they are created. 
	While ulimit is a linux built in command which provides control over the resources available to the shell and/or to processes started by it.

	Q What are inodes in Linux ? How to find the inode associated with a file ?
	Ans = Contence of file will be store in data block .An inode is a data structure on a filesystem on Linux 
	# df -i /root

	Q What is drop cache in Linux and how do you clear it ?
	# echo 1 > /proc/sys/vm/drop_caches = To free pagecache:
	# echo 2 > /proc/sys/vm/drop_caches = To free dentries and inodes:
	# echo 3 > /proc/sys/vm/drop_caches = To free pagecache, dentries and inodes:

	Q:-What is virtual hosting?
	Answer: Virtual hosting is a method for hosting multiple domain names on a server using a single IP address.

	Q: - What is the main difference between <Location> and <Directory> sections?
	Answer:Directory sections refer to file system objects; Location sections refer to elements in the address bar of the Web page

	Q: - How t to enable PHP scripts on your server?
	Answer: If you have mod_php installed, use AddHandler to map .php and .phtml files to the PHP handler. 

	Q: - Which tool you have used for Apache benchmarking?
	Answer: ab (Apache bench)
	ab -n 1000 -c 10 http://www.test.com/test.html

	Q: - Can we cache files which are viewed frequently?
	Answer: Yes we can do it by using mod_file_cache module.
	CacheFile /www/htdocs/index.html

	Q:- On which port Apache listens http and https both?
	# netstat -antp | grep http
	# netstat -anlp |grep 80

	19. What do you understand by MPM in Apache?
	Answer : MPM stands for Multi Processing Modules, actually Apache follows some mechanism to accept and complete web server requests.

	24. What is Loglevel debug in httpd.conf file?
	Answer : With the help of Loglevel Debug option, we can get/log more information in the error logs which helps us to debug a problem.

	25. What’s the use of mod_ssl and how SSL works with Apache?
	Answer : Mod_ssl package is an Apache module, which allows Apache to establish its connection and transfer all the data in a secure encrypted environment.
		How SSL works with Apache
		Apache generates its private key and converts that private key to .CSR file (Certificate signing request).
		Then Apache sends the .csr file to the CA (Certificate Authority).
		CA will take the .csr file and convert it to .crt (certificate) and will send that .crt file back to Apache to secure and complete the https connection request.
		
	Q:- 9. How do I disable directory indexing?
	<Directory />
		Options -Indexes
	</Directory>

	24. How can Apache act a Proxy Server?
	Ans = You can use a mod_proxy module to use as a proxy server. The mod_proxy module can be used to connect 
		to the backend server like Tomcat, WebLogic, WebSphere, etc.

	38. What does 200, 403 & 503 HTTP error code mean?
		200 – content found and served OK
		403 – tried to access restricted file/folder
		503 – the server is too busy to serve the request and in another word – service unavailable.

	Q:9 What is Initrd ?
	Ans: Initrd stands for initial ram disk , which contains the temporary root filesystem and neccessary modules 
		which helps in mounting the real root filesystem in read mode only.
		
	Q:10 What is Bootloader ?
	Ans: Bootloader is a program that boots the operating system and decides from which kernel OS will boot.

	Q:16 How to check all the installed Kernel modules ?
	Ans: lsmod

	Q: 17 What is ITOS in Linux
	Ans: ITOS is built under 32-bit Red Hat Enterprise Linux (RHEL) 5.2, and is tested and supported on standard PCs 
	with x86 processors from Intel and AMD running 32- and 64-bit RHEL 5 and 6. At least 4 GB of RAM is recommended.
	ITOS is a real-time control and monitoring system developed and maintained by a small team at the Goddard Space Flight Center.

	==============================================================================================================================================
	Linux Strucher = User > Shell > Kernel > Hardware
	#visudo     == Note you always use (  visudo not vim /etc/sudoer   )
	#ps -eaf |grep httpd 
	#service --status-all
	cat = Display Content
	less = Display the content of a text file one screen at a time
	more = Display content page by page
	tail = Display last few line of text in file 
	head = display first few line 
	sort  = sort is used for sort data
	tracetout =show network path between location and remote system
	#lsof command = List Open File on linux
	#lsof -u username
	#lsof -i portnumber
	#sync; echo 1 > /proc/sys/vm/drop_caches     ->>> clear butffer cache
	#sync; echo 2 > /proc/sys/vm/drop_caches	 ->>> clear buffer cache
	#echo 3 > /proc/sys/vm/drop_caches			 ->>> clear buffer,swap and cache
	#swapoff -a  then swapon -a 			->>> clear swap memory
	#swapon -a then swapoff -a
	#blkid = Show HardDisk Block ID
	#cat /proc/partition
	#mkfs.xfs -f -L myfs /dev/sda1   >> format xfs File system
	#RAID 0 = Data fast , use total store, no redency
	#RAID 1 = data mirror like use 20 tb you use only 10tb. redency avilabe
	#RAID 5 = striping with parity , Error check available
	#RAID 10 = combination of RAID 0 and RAID 1

	#GRUB File Location > /etc/grub.d  and /etc/default/grub
	if you change in grub file for update use this command
	#grub2-mkconfig -o /boot/grub2/grub.cfg
	if your grub is currpted then follow this steps
	#Got to RecueMode > select recuse mode > Login with root > 
	grub2-mkconfig -o /boot/grub2/grub.cfg  ->> regenrate grub configration file use this command
	if you chage in /etc/default/grub the you use to cd of os then you copy default grub configration

	#root passwd reset
	>>break.rd >>mount -o remount,rw /sysroot >>chroot /sysroot  >> passwd root >>  touch /.autorelabel >> exit >> exit

	===============================================================================================================================================
	Adding New Disk Online On Linux System
	Method 1
	#yum install sg3_utils
	#/usr/bin/scsi-rescan -a 

	Method 2
	#echo "- - - " > /sys/class/scsi_host/hostX/scan
	Note:- hostX is the host controller to be scanned, You can the host controller id.
	#grep mpt /sys/class/scsi_host/host?/proc_name

	Method 3
	lsblk
	lsblk --scsi

	==============================================================================================================================================
	Centralized Log Management Server using rsyslog on RHEL CentOS 7
	#vim /etc/rsyslog.conf
	Uncomment For TCp
	$ModLoad imtcp
	$InputTCPServerRun 514
	ADD Two Line for Each Log
	$temlate TmplAuth, "/var/log/%HOSTNAME%/%PROGRAMNAME%.log"
	*.* ?TmplAuth
	#systemctl restart rsyslog.service 
	https://www.youtube.com/watch?v=f-xNGEU-PZM

	-----
	Fixing Dirty COW on RHEL and CentOS 6.x and 7.x
	The bug is nicknamed Dirty COW because the underlying issue was a race condition in the way kernel handles copy-on-write (COW). 
	#uname -rv
	#wget https://access.redhat.com/sites/default/files/rh-cve-2016-5195_1.sh
	#bash rh-cve-2016-5195_1.sh
	Fix Vulnerability
	sudo apt-get update && sudo apt-get dist-upgrade
	yum update kernel
	sudo reboot
	-----

	Add disk to linux without rebooting the VM
	To find host no: ls /sys/class/scsi_host
	Replace host#  echo "- - -" > /sys/class/scsi_host/host#/scan
	fdisk -l
	#detect a new hard drive attached you need to first get your host bus
	grep mpt /sys/class/scsi_host/host?/proc_name
	----

	Ques.5. What is a Linux Loader or LILO?
	Ans. Linux loader or LILO is a boot loader for Linux that loads the operating system into memory. Now a days, 
	it is considered more of a legacy application superseded by GRUB(GRand Unified Boot loader) and GRUB2 bootloaders.

	Ques.8. What is a process in Linux?

	Ans. A process is an instance of a program under execution. In Linux, there are two kinds of processes-
	Foreground processes - A foreground process when initiated by a user runs in the foreground and user has to wait for it to 
				get completed before issuing any other command e.g. running any command on the terminal.
	Background processes - A background process runs on the background and the user can execute other commands also even before 
				the background process gets executed completely. Adding '&' after any command, makes it a background process. Also, 
				a background process can be brought to foreground using 'fg' command with jobId of the background job.

	https://www.sanfoundry.com/technical-interview-questions/


### how to script file run as service in linux

	Let’s create a file called /lib/systemd/system/rot13.service
	-
	[Unit]
	Description=ROT13 demo service
	After=network.target
	StartLimitIntervalSec=0
	[Service]
	Type=simple
	Restart=always
	RestartSec=1
	User=centos
	ExecStart=/usr/bin/env php /path/to/server.php

	[Install]
	WantedBy=multi-user.target
	-
	You’ll need to:
	set your actual username after User=
	set the proper path to your script in ExecStart=

	---------
	##Encrypting files using 'gpg' - How To Use GPG to Encrypt 
	gpg -c filename    =Encrypting   or gpg -c -a filename
	gpg -t filename    =UNIncrepted file


### Server Hardning By NetworkNuts

	#Set Passwd on Singer Mode 
	vim /etc/sysconfig/init 
	SINGLE=/sbin/sulogin

	#Secure the GRUB using Passwd
	grub-crypt   >>>>Enter
	vim /boot/grub/grub.conf
	Paste Above (tittle Line) 
	password --encrypted Password


### Types of RMAN backups and RMAN components  in mysql 
	FullBackup=Entire database back
	Incremental = FullBackup + Backup change only lastback = but they only back up the data that has changed since the last backup
					Friday - Full Backup, Monday - All changed files since Friday, Tuesday - All changed files since Monday
	Differntial = A differential backup backs up only the files that changed since the last full backup.
					Friday - Full Backup, Monday - All changed files since Friday, Tuesday - All changed files since Friday, including Monday's changed files


### What is latency and why is it such a big deal?  WEB SERVER Site.....................
	In web performance circles, “latency” is the amount of time it takes for the host server to receive and 
	process a request for a page object. The amount of latency depends largely on how far away the user is 
	from the server.
	1.Enable Caching
	2.Optimize and Reduce the Size of Your Images
	3.Minimize Your Code
	4.Use a CDN
	5.Excessive usage of social media scripts:
	6.Too Much Flash Content
	7.Excessive HTTP Requests   >> When a user visits your web page, the browser performs several requests to load each of these files
	8.Unnecessary redirects


### What is patching & updating in Linux & rollback updates.
	#yum history
	#yum history
	#yum history rollback idNumber


### Q = linux round robin

	Round Robin DNS is a technique in which load balancing is performed by a DNS server instead of a strictly dedicated machine. A DNS record has more 
	than one value IP address.When a request is made to the DNS server that serves this record, the answer it gives alternates for each request.
	DNS round-robin is mapping multiple servers to the same hostname, so that when users visit foo.example.com multiple servers are available to handle their requests.

	When you have server back ends built of multiple servers, such as clustered or mirrowed web or file servers, a load balancer provides a single point of entry. 
	Large busy shops spend big money on high-end load balancers that perform a wide range of tasks: proxy, caching, health checks, SSL processing, 
	configurable prioritization, traffic shaping, and lots more.

	But you don't want all that. You need a simple method for distributing workloads across all of your servers and providing a bit of failover and don't 
	care whether it is perfectly efficient. DNS round-robin and subdomain delegation with round-robin provide two simple methods to achieve this.



				























































