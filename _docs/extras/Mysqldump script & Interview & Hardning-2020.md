---
title:  "Mysqldump script & Interview & Hardning-2020"
subtitle: "It's always a bit messy"
author: "Shamsher Kushwaha"
avatar: "img/authors/43068991.png"
image: "img/apache-security-hardening-guide.png"
date:   2020-11-29 20:51:12
---
## Mysqldump
 
    mysqldump -u root -psonu12 --database bugs > /home/bugs-`date +%d-%m-%y`.sql

    iptables -I INPUT -p tcp -i eth0 -s 203.92.34.189 --dport 8052 -j ACCEPT

    */1 * * * * /bin/sh /home/mysql.sh
    45 13 * * * /bin/sh /home/mysql.sh

## Mysql load Moniter

    4 Useful Commandline Tools to Monitor MySQL Performance in Linux


    * 1> Mytop Tools or Mtop Tools
    * 2> # sar 2 5  ,# mysql> show full processlist;



## MySQL performance tuning Mysql Tunning Best

    vim /etc/my.cnf
 
    innodb_buffer_pool_size=1024M
    innodb_log_file_size = 32M
    innodb_log_buffer_size = 4M
    query_cache_size=64M
    key_buffer        = 300M
    max_allowed_packet    = 16M
    thread_stack        = 128K
    thread_cache_size    = 384
    max_connections        = 400
    table_cache            = 1800
    tmp_table_size = 64M
    max_heap_table_size = 64M
    max_connect_errors = 1000
    wait_timeout = 7200
    connect_timeout = 20
    #(sleeping connections)
    wait_timeout = 60
    max_connections = 400
    key_buffer = 128M
    #(Query Cache)
    query_cache_size = 128MB
    query_cache_limit = 4MB
    #(Temporary tables)
    tmp_table_size = 64MB
    table_cache = 512

## MYSQL ALL COMMAND 


    #service mysqld stop
    mysqld_safe --skip-grant-tables &    ==>> For reset mysql password
    mysql
    mysql> flush privileges;
    mysql> SET PASSWORD FOR root@'localhost' = PASSWORD('sonu12');
    mysql> flush privileges;

## USER ADD

    mysql_secure_installation

    How to Create a New User
    #CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
    #GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'localhost';
    #FLUSH PRIVILEGES;


    #CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
    #GRANT ALL PRIVILEGES ON mydb.* TO 'myuser'@'%' WITH GRANT OPTION;
    #FLUSH PRIVILEGES;

    #Creating a read-only database user account for MySQL   AND FULL Access.
    grant SELECT,SHOW VIEW on database_name.* to 'read-only_user_name'@'%' identified by 'password';
    GRANT ALL PRIVILEGES ON *.* TO 'username'@'localhost' IDENTIFIED BY 'password';

    #grant all privileges on qa_sforskill.* to 'root'@'192.168.10.%' identified by 'MTC2012QWER!@#$';

## 100GB BACKUP Mysql Server 

    cp -r /var/lib/mysql/  /home/sonu
    mysqldump -u root -p[pass] [dbname] | gzip -9 > backupfile.sql.gz

## Others Commands

    #use cloud;   = switch to a database
    #show tables; = get the list of all the tables
    #describe oc_users; = get the Field Name
    #drop table lookup; = delete a table
    #drop database a1; = delete a database

    #mysqladmin -h 172.16.25.126 -u root -p 		= Remote Access Mysql data server
    #mysqladmin -u root password YOURNEWPASSWORD 	= set MySQL Root password
    #mysqladmin -u root -p123456 password 'xyz123' 	= Change MySQL Root password
    #mysqladmin -u root -p processlist 				= check all the running Process of MySQL server



### Backup and Restore

    # mysqldump -u [root] -p[pass] [db_name > db_backup.sql
    # mysqldump -u [root] -p[pass] [--all-databases] > all_db_backup.sql
    # mysqldump -u [root] -p[pass] [db_name] table1 table2 > table_backup.sql
    # mysqldump -u [root] -p[pass] [db_name] | gzip > db_backup.sql.gz      ()
    # mysqldump -u [root] -p[pass] [dbname] | gzip -9 > [backupfile.sql.gz]    (with Compress)
    # mysqldump -P 3306 -h [ip_address] -u [uname] -p[pass] db_name > db_backup.sql
    $ mysql -u username -p -h 202.54.1.10 databasename < data.sql


    # mysqladmin -u root -p kill 5,10
    # mysqladmin -u root -p processlist
    # mysqladmin  -u root -p debug
    # mysqladmin  -u root -p start-slave
    # mysqladmin  -u root -p stop-slave
    # mysqladmin  -u root -p debug


### Crash Recovering a Mysql Database after Crash+++++++++++++++++++++++++++++++++++++++++

    #/etc/init.d/mysql stop
    #copy the binaries  =>default mysql databases are located in /var/lib/mysql --> each folder is the database name
    #/etc/init.d/mysql restart
    #scp -r sonudbname root@192.168.10.182:/var/lib/mysql

### Check and Repair a Corrupted MySQL 

    #mysqlcheck -r [database name]
    #myisamchk -r /var/lib/mysql/[database name]/*
    #myisamchk --recover TABLE


### Running the InnoDB recovery process

    #vim my.cnf file
    Add the following line to the [mysqld] section:
    #innodb_force_recovery=4      {after recover table #Comment this Line}


### mysql_secure_installation

    (VVIP) Diffrent between AWS and Local Apache Server Important must Disabled this line #sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES   (in mysql /etc/mysql.cnf) (vvip) innodb_large_prefix=1



### Process List Check 

    mysql -u root -p processlist; ====>>> SHOW FULL PROCESSLIST 
    ===>>kill <thread_id>;
    MySQL ‘show status’ and open database connections = show status like 'Conn%';

### Other Commands

    Change Passwd = mysql -u root passwdord newpassword;
    Ping Mysql  = mysql -u root -p ping;
    Version Check =  mysql -u root -p version;
    Status mysql = mysql -u root -p status;
    Check Variable = mysql -u root -p variables;
    Row check  = Show table = Select * FROM TABLE NAME

    mysql> SELECT GROUP_CONCAT(CONCAT('KILL ',id,';') SEPARATOR ' ') 'Paste the following query to kill all processes' FROM information_schema.processlist WHERE user<>'system user'\G


### How to view all running processes in Linux Mint

    Shift+m
    shift+k than type Process ID NUMBER

    Find out which process is listening upon a port
    lsof -i
    netstat -tulpn | less 
    netstat -tulpn | head 
    netstat -a | less   ====>Display All connectio TCP and UDP
    netstat -g   ==  ALL NETWORK Enthernet
    netstat -l ===>>>List Only Listening Port

    lsof -i :22
    lsof -p 1192 | grep bin
    lsof -i tcp  === lsof -i udp

    nmap -sT -0 localhost  ===>> 
    ps aux | grep httpd

    hostname -d, hostname -f, hostname -i 

### TOP COMMAND =====

    top
    SHIFT +M  => List the process accourding memory use
    SHIFT +P  => List the process accourding CPU use
    SHIFT +W  => For saving the top output in a file (/root/.toprc)
    SHIFT +O  => For sorting the process as per requirements...

    C= List running process with absolute path
    K= for killing the process
    z= List running process in color
    r= For renice a process
    1(number)= Show the details of individual cps running on the system
    d= Change refresh rate
    A= Splite top output into multiple screen
    h= For help
    n= Reduce the number of listed process
    l= To hide and show load average
    t= To hide and show load cpu status
    m= To hide and show load memory information_schema

###  PAGERS AND OUTPUT COMMANDS==================================

    more = Splits output into page & navigation
    less = Splits output into page & navigation	
    head = Display first few lines at the top of a file
    tail = Display the last few lines of a files

### VIM EDITOR===========================================================

    dd=For Deleting
    yy=For Copying
    p=For Pest
    :r filename= For Reading Text from another file
    /search-text= For searching top to bottom 
    ?search-text= For searching bottom to top
    n= For following the search text top to bottom
    N= For following the search text top to bottom
    O or o = For A New line above or below the current line
    r- =For Replacing the text under curser.
    $= For end of the line.
    ^= For Beginning of the line.
    :1= For the First Line.
    G= For the last line.
    :n= For the nth Line NUMBER

### GREP Command==========================================================

    grep "this" demo_file
    grep -i "string" demo_file
    grep -iw "is" demo_file
    grep -A 3 -i "example" demo_file
    grep -r "sonu" *
    grep -r "sonu" * |wc -l
    grep -c "sonu" demo_file


### Find COMMAND  =====================================================
    find / -name file_name
    find . -type f -perm 0777 -print  == File find based on their Permission(777)
    find / -type -f -perm 0777 -exec chmod 755 {} \; = Change file permission
    find / -mtime 10 = Find all file that were modified 10 days backup
    find / -mmin 1 = 
    find . -type f ! -perm 0777 -print == File find without  Permission 
    find / -perm 2755 == Find SGID File with permission 755
    find / -perm /g+s == Find all 	SGID set file
    find / -perm 1755 == File sticky bit file with permission 755
    find / -user root -name abc.txt = = find a file based on user
    find / -size 10M == Find a 10 MB 
    find / -size +10M -size -30M  == Find all File between 10MB and 30MB
    find / -maxdepth 3 -name "*log"
