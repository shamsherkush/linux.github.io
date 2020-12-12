

### Yum install php httpd gd* gd-devel gcc* glibc glibc-common wget perl parl-devel make net-snmp glib* -y 
	# useradd –m nagios
	# passwd nagios
	# groupadd nagcmd
	# useradd –a –G nagcmd nagios
	# useradd –a –G nagcmd apache 
	#mkdir nagios 
	#cd nagios
	Download nagios 4.0.8 in this directory && Download nagios-plugins 2.0.3  in this directory
	#Tar –xvf  nagios 4.0.8 and nagios-plugins 2.0.3

	#Cd  nagios 4.0.8
	# ./configure –with-command-group-nagcmd
	# make all 
	# make install 
	# make install-init 
	# make install-config
	# make install-commandmode
	# make install-webconfig


### vim /usr/local/nagios/etc/object/contacts.cfg
	Change Email - shamsherkush@gmail.com  


	Nagious Admin UserName Create For Web
	........
	#htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
	#Passwd= mount@12345
	.................
	#Service httpd restart   >>>> chkconfig httpd on 
	>>>>>>>>>>>>>>>>>>>
	# cd nagios-plugins-2.0.3
	# ./configure --with-nagios-user-nagios --with-nagios-group-nagcmd
	# make 
	# make install 
	# vim /etc/httpd/conf.d/nagios.conf
	#Allow from all  Block
	Allow from 127.0.0.1 192.168.10.0/24 open 
	# service httpd restart
	===========================================================================================================

	For Find Error or Syntex is ok   (VVIP)
	/usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg   (to check all nagios setting ok or not)

	===========================================================================================================
	#Service  nagios start
	#chkconfig nagios on
	>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>.>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
	192.168.10.201/nagios
	User=nagiosadmin  passwd=mount@12345
	>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

	Take backup of configratun file 
	#cp -p /usr/local/nagios/etc/nagios.cfg /usr/local/nagios/etc/nagios.cfg_backup
	# Vim nagios.cfg
		cfg_dir=/usr/local/nagios/etc/servers   unblock cfg_directory line and save
		status_update_interval=01

	#mkdir  /usr/local/nagios/etc/servers   (make directory on location)
	#vim /usr/local/nagios/etc/servers/clients.cfg  (make own cfg file)

	
	########################Amit####################################################################################
	#vim /usr/local/nagios/etc/object/localhost.cfg   (command.cfg) , (switch.cfg), (windos.cfg), (templates.cfg)      


	define host{
	use                     linux-server
	host_name               mtcdev01
	alias                   Vertual_Server
	address                 192.168.10.77
	max_check_attempts      5
	check_period            24x7
	notification_interval   30
	notification_period     24x7
	}
	>>>>>>>>>>>>>>>>>>>>>>>>Group Host>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
	#######################Development Group ######################################################################
	# Define an optional hostgroup for Linux Servers
	define hostgroup {
					hostgroup_name          Development-Users ; GroupName
					alias                   Development-Linux-Server

	}

	########################Amit###################################################################################
	define host{
	use                     linux-server
	host_name               mtcdev01
	alias                   Vertual_Server
	address                 192.168.10.77
	max_check_attempts      5
	check_period            24x7
	notification_interval   30
	notification_period     24x7
	hostgroups              Development-Users
	}

	>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>.>>>
	Go to Client And setting chage    (Monitor a CentOS 7 Host with NRPE)

	#yum install nrpe nagios-plugins-all
	#vim /etc/nagios/nrpe.cfg
		allowed_hosts=127.0.0.1,10.132.224.168
		
	#systemctl start nrpe.service
	#systemctl enable nrpe.service

	Add Host to Nagios Configuration
	#vim /usr/local/nagios/etc/servers/yourhost.cfg
		define host {
			use                             linux-server
			host_name                       yourhost
			alias                           My first Apache server
			address                         10.132.234.52
			max_check_attempts              5
			check_period                    24x7
			notification_interval           30
			notification_period             24x7
		}
	#systemctl reload nagios.service



	==============================================================================================================
	==========================Nagios InterView Question========Nagios File and location===========================
	==============================================================================================================

	Log_file = /usr/local/nagios/var/nagios.log  
	Check Nagioe Setting = /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
	Main_ConfiFile= /usr/local/nagios/etc/nagios.cfg

	Object_ConfigFile= 		
		cfg_file=/usr/local/nagios/etc/hosts.cfg,
		cfg_file=/usr/local/nagios/etc/services.cfg,
		cfg_file=/usr/local/nagios/etc/commands.cfg
	Object Config Directory= 
		cfg_dir=/usr/local/nagios/etc/commands, 
		cfg_dir=/usr/local/nagios/etc/services, 
		cfg_dir=/usr/local/nagios/etc/hosts
	Object Cache File = 
		object_cache_file=/usr/local/nagios/var/objects.cache
		Precached Object File= precached_object_file=/usr/local/nagios/var/objects.precache 

	Status File= status_file=/usr/local/nagios/var/status.dat
	Log Archive Path = log_archive_path=/usr/local/nagios/var/archives/
	External Command File = command_file=/usr/local/nagios/var/rw/nagios.cmd
	Lock File =  lock_file=/tmp/nagios.lock
	State Retention File=  state_retention_file=/usr/local/nagios/var/retention.dat
	Check Result Path =    check_result_path=/var/spool/nagios/checkresults
	Host Performance Data File =  host_perfdata_file=/usr/local/nagios/var/host-perfdata.da
	Service Performance Data File =  service_perfdata_file=/usr/local/nagios/var/service-perfdata.dat
	Debug File = debug_file=/usr/local/nagios/var/nagios.debug

#  QA

	Q What is Nagios?
	An = Nagios is one of the monitoring tools.

	Q What is NRPE (Nagios Remote Plugin Executor) in Nagios?
	An = The NRPE addon is designed to allow you to execute Nagios plugins on remote Linux/Unix machines.

	Q What do you mean by passive check in Nagios?
	An = Passive checks are initiated and performed by external applications/processes and the Passive check results are submitted to Nagios for processing.

	Q What is the difference between Active and Passive check in Nagios?
	An = Active checks are initiated by the check logic in the Nagios daemon.,while passive checks are performed by external applications.
		Passive checks Active checks are initiated by the Nagios process.
		Passive check are run on a regularly scheduled basis.

		
	Q What Are Ports Numbers Nagios Will Use To Monitor Clients?----------------------------------------
	An = Port numbers are 5666, 5667 and 5668

	Q What Is Ndoutils ?
	An = The NDOUTILS addon is designed to store all configuration and event data from Nagios in a database.
		Storing information from Nagios in a database will allow for quicker retrieval and processing of that data

	Q Explain Main Configuration File And Its Location?-----------------------------------------------
	An = Resource File : It is used to store sensitive information like username, passwords 
		with out making them available to the CGIs. 
		Default path: /usr/local/nagios/etc/resource.cfg
		
		Object Definition Files: It is the location were you define all you want to monitor and how you want to monitor.
		It is used to define hosts, services, hostgroups, contacts, contact groups, commands, etc.. 
		Default Path:/usr/local/nagios/etc/objects/ ----------------------------
		
		CGI Configuration File : The CGI configuration file contains a number of directives that affect the operation of the CGIs.
		It also contains a reference the main configuration file, so the CGIs know how you’ve configured Nagios and where your object definitions are stored.
		Default Path: /usr/local/nagios/sbin/

	Q  Nagios Administrator Is Adding 100+ Clients In Monitoring But He Don’t Want To Add Every .cfg File Entry In Nagios.
		cfg File He Want To Enable A Directory Path. How Can He Configure Directory For All Configuration Files?
	Answer : He can able to achieve the above scenario by adding the directory path in nagios.cfg file, in line number 54 we have to add below line.
			54  cfg_dir=/usr/local/nagios/etc/objects/monitor
			
	Q How To Verify Nagios Configuration?
	Ans = /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg   ---------------------

	Q What Is The Difference Between Nagiosxi And Nagios Core?
	An = NagiosXI is a Paid version and Nagios core is a free version.

	Q What Is Database Is Used By Nagios To Store Collected Status Data?
	An = Nagios core will use default RRD database format to store status data

	Q . What are state types of Nagios?
	An =	Service or host state type
			Some states like OK, WARNING, UP, or DOWN state host or service
			Two state types that are SOFT state or HARD state

	Q. What are SOFT and HARD states?
	An = In case of the SOFT state, the service or host check results are not OK or not up to the mark,
		When a host or service check result is not ‘OK’ and it has been checked for the number of times, 
		
	Q What is state stalking in Nagios?
	An = State stalking is used for logging purpose in Nagios. When stalking is enabled for any service 
		host then Nagios watch it very carefully and store any changes that if found in the check result of that resource.
		
	Q Which three Nagios variables can affect recursion and inheritance in Nagios?
	An = The three variables that affect recursion and inheritance are:
			Name
			Use
			Register
			
	Q What is meant by saying Nagios is Object Oriented ?
	An = One of the features of Nagios is object configuration format in that you can create object definitions that inherit properties
		from other object definitions and hence the name. This simplifies and clarifies relationships between various components.
		
	Q How To Generate Performance Graphs ?
	An = In Nagios Core there is no inbuilt option to generate the performance graphs, We have to install pnp4nagios 
		and add hosts and services URL’s in defination files.
		
	Q 4. Nagios administrator is adding 100+ clients in monitoring but he don’t want to add every .cfg file entry in nagios.cfg file he want to enable a directory path. How can he configure directory for all configuration files..?
	Ans: He can able to achieve the above scenario by adding the directory path in nagios.cfg file, in line number 54 we have to add below line.

	54  cfg_dir=/usr/local/nagios/etc/objects/monitor

	9. What Are Objects?
	Ans:Objects are all the elements that are involved in the monitoring and notification logic.
		Types of objects include:- Services, Hosts, Host Groups, Contacts, Contact Groups , Commands, Time Periods, Notification Escalations.
	
























