#                                                           Linux Interview Questions  

 

#   1- LAMP Stack: -  

    Q: - Can you execute a PHP script in command line? 

    A:-  Yes we can execute a PHP script in command line. | php yourscript.php. 

    

    Q: - How to repair a table in MySQL? 

    A: - To repair a table in MySQL you need to use the following query: REPAIR TABLE {table name}; 

    

    Q: - Please explain how can you take a backup of the whole database in mysql? 

    A: - You can use the command line utility to take a backup of all the mysql databases:  mysqldump -u user -p --all-databases > all-databases.sql 

    

    Q: - if your apache service is not running what will you so? 

    A: - check the logs and resolving the issue. 

    

    Q: - when access the website if coming 403 forbidden error how to resolve? 

    A: - check the logs and resolving the issue. And check also website folders and files 

    

    Q: - when access website home page is working but inner page on show error HTTP 404 how to resolve? 

    A: - check website .htaccess file rule and check rewrite module on apache enable or not. 

    

    Q: - suppose you installed php on Linux server but when run the command php then error show is command is not found or php is not installed on server so what you do?  

    A: - check php-cli module installed or not on server if yes then check php binary if fund then link the /bin/php  

    Q: - how create daily or weekly auto backup source code and databases in Linux and put the backup 30 days on aws s3? 

    A: - create a bash or python script for backup and put the backup on s3 and define retention period in bash or python script. 

    

### SSL Setup -  

    

    Q: - explain the SSL free or paid setup on Linux server. 

    A: - for a paid first generate CSR then put CSR on your SSL panel like GoDaddy then download the ssl and installed on SSL VHOST file.  

    For Free install the lets encrypt on Linux server or another free tool then configures the ssl. 

    

    Q: - How Many types of SSL? 

    A: - Extended Validation (EV SSL) Certificates. 

    Organization Validated (OV SSL) Certificates. 

    Domain Validated (DV SSL) Certificates. 

 

### NODE AND MONGODB - 


    Q: - What is the node server? 

    A: - Node.js is an open-source server environment 

    Node.js is free 

    Node.js runs on various platforms (Windows, Linux, Unix, Mac OS X, etc.) 

    Node.js uses JavaScript on the server. 

    

    Q: - what the is the use of pm2 module on node server please explain? 

    A: - PM2 is a production process manager for Node.js applications with a built-in load balancer. It allows you to keep applications alive forever, to reload them without downtime and to facilitate common system admin tasks. 

    Starting an application in production mode is as easy as: 

    $ pm2 start app.js 

    

    Q: - what is the mongo dB? 

    A: - MongoDB is a document-oriented NoSQL database used for high volume data storage. Instead of using tables and rows as in the traditional relational databases, MongoDB makes use of collections and documents. 

    

    Q: - which is the type of mongo dB database collection stored on Linux? 

    A: - JSON. 

    

### Linux server- 

    Q: - What is the Linux server? 

    A: - A Linux server is a server built on the Linux open-source operating system. It offers businesses a low-cost option for delivering content, apps and services to their clients. Because Linux is open-source, users also benefit from a strong community of resources and advocates. 

    

    Q: - explain the boot process of Linux os. 

    A: - 6 Stages of Linux Boot Process (Startup Sequence) 

    1. BIOS 

    2. MBR 

    3. GRUB 

    4. Kernel 

    5. Init 

    6. Run level programs 

    

    

    

    Q: - what is the use of fsck command in Linux. 

    A: - The fsck (File System Consistency Check) Linux utility checks filesystems for errors or outstanding issues. The tool is used to fix potential errors and generate reports. 

    

    Q: - What are Hard Links? 

    A: - 1. Hard Links have the same inodes number. 

    2- You cannot create a Hard Link for a directory. 

    3. Links have actual file contents 

    

    Q: - What are Soft Links? 

    A: - 1. Soft Links have different inodes numbers. 

    2. A Soft Link can link to a directory. 

    3. Soft Link contains the path for original file and not the contents. 

    

    Q: - what is the use df- h? 

    A: - You can use -h with df only to produce the output in readable format for all the mounted file systems rather than just of the file system. 

    

    Q: - How many types of file systems in Linux? 

    A: - ext2, ext3, ext4, jfs, ReiserFS, XFS, Btrfs. 

    

### JENKINS Interview 

    

    

    Q: - Explain what is continuous integration? 

    A: - In software development, when multiple developers or teams are working on different segments of same web application, we need to perform integration test by integrating all modules.  In order to do that an automated process for each piece of code is performed on daily bases so that all your code get tested. 

    

    Q: - What is the requirement for using Jenkins? 

    A: - To use Jenkins you require 

    A source code repository which is accessible, for instance, a Git repository 

    A working build script, e.g., a Bash script, checked into the repository 

    

    Q: - how many types of pipeline in Jenkins? 

    A: - A Jenkins file can be written using two types of syntax - Declarative and Scripted. Declarative and Scripted Pipelines are constructed fundamentally differently. Declarative Pipeline is a more recent feature of Jenkins Pipeline which: provides richer syntactical features over Scripted Pipeline syntax, 

    

    Q: - Jenkins modules already install or not. 

    A: - Not. 

    

    Q: - how types of modules activation on jenkins? 

    A: - Two type restart and no restart the Jenkins after activating. 

    

    Firewalls-  

    Q: - what is the work of firewall in Linux. 

    A:- A firewall is a system that provides network security by filtering incoming and outgoing network traffic based on a set of user-defined rules. In general, the purpose of a firewall is to reduce or eliminate the occurrence of unwanted network communications while allowing all legitimate communication to flow freely. 

    

    Q: - what is the iptables in Linux? 

    A:- On the one hand, iptables is a tool for managing firewall rules on a Linux machine. 

    

    Q: - What is the dir path of iptables config files? 

    A:- The iptables service stores configuration in /etc/sysconfig/iptables and /etc/sysconfig/ip6tables , while firewalld stores it in various XML files in /usr/lib/firewalld/ and /etc/firewalld. 

    

    Q: - what is the command of show the iptables rule List? 

    A:- iptables -L 

    

    Q: - what is the command of flush the iptables rules? 

    A:- iptables -F 

    

# How do I find all of the symlinks in a directory tree? 

A: find . -type l -ls 

 
