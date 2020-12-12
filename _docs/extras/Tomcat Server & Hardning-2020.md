---
title:  "How to Install and Configure Apache Tomcat 9 in CentOS 8/7"
subtitle: "It's always a bit messy"
author: "Shamsher Kushwaha"
avatar: "img/authors/43068991.png"
image: "img/apache-security-hardening-guide.png"
date:   2020-11-29 20:51:12
---
## How to Install and Configure Apache Tomcat 9 in CentOS 8/7

### Apache Tomcat is an open-source web server for a Java-based web application.

	# yum install epel-release
	# yum install java-1.8.0-openjdk-devel -y OR  yum install java-1.8.0-openjdk.x86_64   OR dnf install java-11-openjdk
	# java -version
	# javac -version


### Step 2: Installing Apache Tomcat 9

	# cd /usr/local
	# wget https://downloads.apache.org/tomcat/tomcat-9/v9.0.38/bin/apache-tomcat-9.0.38.tar.gz
	# tar -xvf apache-tomcat-9.0.26.tar.gz
	# mv apache-tomcat-9.0.26 tomcat9



### Before starting the Tomcat Service, configure a CATALINA_HOME environment variable. Set Apache Tomcat CATALINA_HOME

	# echo "export CATALINA_HOME="/usr/local/tomcat9"" >> ~/.bashrc
	# source ~/.bashrc



###  Start and Stop  the tomcat web server using the scripts provided by the tomcat package.

		# cd /usr/local/tomcat9/bin
		# ./startup.sh   & ./shutdown.sh


	http://Your-IP-Address:8080   =  URL Access  = Access Main Page


###  Configuring Apache Tomcat 9  Set (USER NAME & PASSWORD)  FOR ACCESS   (Server Status & Manager App & Host Manger)

	# vim /usr/local/tomcat9/conf/tomcat-users.xml 
		<role rolename="admin-gui"/>
		<role rolename="manager-gui"/>
		<user username="admin" password="good_password" roles="admin-gui,manager-gui"/>
		


	

### Enable Remote Access to Tomcat. You have to Change on Both Location. (First in manager & Second host-manager)

	cd /usr/local/tomcat9/webapps/manager/META-INF/context.xml					= allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1|192.168.1.*" />
	cd /usr/local/tomcat9/webapps/host-manager/META-INF/context.xml    			= allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1|192.168.1.*" />



### Changing Apache Tomcat Port  8080 to 80   =  (Tomcat Configuration File)

	cd /usr/local/tomcat9/conf/server.xml
		<Connector port="80" protocol="HTTP/1.1"      (Change port 8080 to 80)
	




### vim /usr/local/tomcat9/conf/tomcat-users.xml
	<!-- user manager can access only manager section -->
	<role rolename="manager-gui" />
	<user username="manager" password="good_password" roles="manager-gui" />

	<!-- user admin can access manager and admin section both -->
	<role rolename="admin-gui" />
	<user username="admin" password="good_password123" roles="manager-gui,admin-gui" />







	cd /usr/local/tomcat9/bin		=	This contains all the binary files
	cd /usr/local/tomcat9/conf		=	This is the tomcat configuration directory
	cd /usr/local/tomcat9/lib		=	Contains library files and classes that are required for the tomcat server
	cd /usr/local/tomcat9/logs		=	Contains the log and output files of tomcat 
	cd /usr/local/tomcat9/webapps	=	This is where you’ll deploy your java web application.
	cd /usr/local/tomcat9/work		=	This is used as a temp working directory for your web apps.
	cd /usr/local/tomcat9/temp		=	This is used by the JVM, which will create any required temp files


	dmidecode	=	Check Your Server HardWare


######  Tomcat Install SSL Certificate   on 8443 Port=

	Tomcat Install SSL Certificate   on 8443 Port

	##Generating a Self-Signed SSL Certificate:  
	(generate a JKS certificate file)
		#mkdir /etc/keys
		#cd /etc/keys
		#keytool -genkey -alias tomcat -keyalg RSA -keystore sonu.sonu.com.jks     

	NOTE: Here, tomcat is the alias of the Java KeyStore file. You can change it to anything you want. Also, 
	sonu.sonu.com.jks is the name of the output JKS certificate file.


	NOTE: If you want to use wildcard domain names, you may do so here. For example, you can type in *.yourdomain.com; In that case, this certificate will 
	be valid for sonu.sonu.com, sonu2.sonu.com and so on.


	##Installing Self-Signed SSL Certificate on Tomcat Web Server:
	#vim /usr/local/tomcat9/conf/server.xml

	<Connector port="8443" maxThreads="150"
			scheme="https" secure="true" SSLEnabled="true"
			keystoreFile="/etc/keys/sonu1.sonu1.com.jks" keystorePass="123456"
			clientAuth="false" keyAlias="tomcat" sslProtocol="TLS"/>

	Before This Line <!-- Define an SSL/TLS HTTP/1.1 Connector on port 8443


	#restart Tomcat Server  # URL = https://sonu1.sonu1.com:8443/


### Hardening and Security of Tomcat Server 

	1.) Remove Server Banner
		<Connector port="8080" protocol="HTTP/1.1"
		connectionTimeout="20000"
		Server =" "
		redirectPort="8443" />
		
	2.) Starting Tomcat with a Security Manager
		[root@geekflare bin]# ./startup.sh -security
		
		
	3.) Enable SSL/TLS
		SSLEnabled="true" scheme="https" keystoreFile="ssl/bloggerflare.jks" keystorePass="chandan" clientAuth="false" sslProtocol="TLS"
		
		
	4.) Enforce HTTPS
		This is only applicable when you’ve SSL enabled. If not, it will break the application.
		Once you’ve enabled SSL, it would be good to force redirect all HTTP requests to HTTPS for secure communication between user to Tomcat application server.
		#Go to $tomcat/conf folder
		#Modify web.xml by using vim
		#Add following before </web-app> syntax
		
		<security-constraint>
		<web-resource-collection>
		<web-resource-name>Protected Context</web-resource-name>
		<url-pattern>/*</url-pattern>
		</web-resource-collection>
		<user-data-constraint>
		<transport-guarantee>CONFIDENTIAL</transport-guarantee>
		</user-data-constraint>
		</security-constraint>


	5.) Add Secure & HttpOnly flag to Cookie
		This is done by adding below the line in session-config section of the web.xml file
		
		<cookie-config>
		<http-only>true</http-only>
		<secure>true</secure>
		</cookie-config>
		
		
	6.) Run Tomcat from non-privileged Account
		useradd tomcat
		chown -R tomcat:tomcat tomcat/
		
		
	7.) Remove default/unwanted Applications
		ROOT – Default welcome page
		Docs – Tomcat documentation
		Examples – JSP and servlets for demonstration
		Manager, host-manager – Tomcat administration
		#They are available under $tomcat/webapps folder
		
		
	8.) Change SHUTDOWN port and Command
		<Server port="8005" shutdown="SHUTDOWN">     replace to   <Server port="8867" shutdown="NOTGONNAGUESS">
		
		
	9.) Replace default 404, 403, 500 page
		

	10.) Securing Manager WebApp
		<role rolename="manager"/>
		<user username="darren" password="ReallyComplexPassword" roles="manager"/>
		

	11.) Disable sending of the X-Powered-By HTTP header

	12.) Disable support for TRACE requests


	
# 									QA

	Q #1) What is Apache Tomcat?
	Answer: Apache Tomcat is basically a Web Server and Servlet system which is open source (i.e. freely available on the internet) and is created 
			by Apache Software Foundation. Apache Tomcat is the server which is mostly used by Java Developers.
			There are basically two types of server:
				1. Application Server
				2. Web Server
				
	Q #2) Why do we require Apache Tomcat?
	Answer: Apache Tomcat is required to run Java Web Applications on the host and server-based system. It also helps to run JSP and Servlets.


	Q #3) What is the default port for Apache Tomcat?
	Answer: The default port of Apache Tomcat is port 8080.


	Q #4) What is the name of inbuilt Web Container in Tomcat?
	Answer: The name of the inbuilt Web Container in Tomcat is Catalina which is present in the bin directory.


	Q #5) What are the types of batch file with the help of which we can Start and Stop Apache Tomcat Server?
	Answer: There are basically two types of batch file with which we can Start and Stop Apache Tomcat Server.
		They are as follows: Startup.bat   and  Shutdown.bat
		
	Q #10) Explain the types of connectors used by Apache Tomcat.
	Answer: Apache Tomcat basically uses two types of connectors which are as follows:
			HTTP Connectors:= HTTP connectors possess attributes which can be modified to determine exactly how it works and access functions such 
								as redirects and proxy forwarding.
			
			AJP Connectors:= AJP connectors follow the AJP protocol in place the of HTTP but work just same as HTTP connectors. They are implemented 
							in Apache Tomcat through the plug-in technology mod_jk.
							
							
							
	Q #11) Mention the configuration files of Catalina.
	Answer: The configurations files of Catalina include:
			( XML, Properties, Policy, Tomcat-users.xml )
			

	Q #13) What is the deployment process of web application using WAR file?
	Answer: There is a Web apps directory in Tomcat under which all the web components JSP, Servlets, HTML are placed. Hereby putting all the files 
			into a single folder we can compress the files into a single unit which has .WAR extension.
			Now we can easily deploy the web application by putting the WAR file in the Web apps directory.
			And when the server starts it extracts all the web components.



	Q #14) What is the functionality of Tomcat Valve?
	Answer: Tomcat Valve is a new feature which was introduced with Tomcat 4.
			Tomcat Valve is used to link an object of Java class with a specific container of Catalina.



	Q #24) How is Apache Tomcat different from Apache Web Server?
	Answer: Apache Tomcat is used to host the web contents whereas Apache Web server is an HTTP server that is built to serve the static contents.


	Q #33) How can we put a restriction to upload files on our web server?
	Example: LimitRequestBody 20000


	Q #35) What is the significance of status code 403 and 404 in Apache Server?
	Answer: The significance of Status code 403 and 404 are mentioned below:
			Status code 403:=  It refers to a forbidden error like if a file misses some security context.
			Status code 404:=  It refers to an error message that it is an http response and the client was not able to communicate with the given server.

	5. Where can be set roles, username, and password?
	Answer:  /conf/tomcat-users.xml



	6. What Is Default Session Time Out In Tomcat?
	Answer:  The default session timeout 30 minutes in tomcat and can change in $TOMCAT_HOME/conf/web.xml via modifying below entry.


	7. what is the configuration file in tomcat server?
	Answer: Tomacat XML Configuration Files:
		server.xml(TOMCAT-HOME/conf/server.xml)
		web.xml (TOMCAT-HOME/conf/web.xml)
		tomcat-users.xml (TOMCAT-HOME/conf/tomcat-users.xml)


	8. How to change the default(8080) port number?
		Edit server.xml. (/usr/share/tomcat/conf/server.xml)


	22. What is the web.xml configuration file?
	Answer: The web.xml file is derived from the Servlet specification and contains information used to deploy and configure the components of 
			your web applications.


	23. What is Coyote?
	Answer: Coyote is a Connector component for Tomcat that supports the HTTP 1.1 protocol as a web server. This allows Catalina, nominally a 
			Java Servlet or JSP container, to also act as a plain web server that serves local files as HTTP documents.
			
			
	28. What is Catalina?
		Answer: Catalina is Tomcat’s servlet container. Catalina implements specifications for servlet and JavaServer Pages. Catalina is the Java 
		Engine (JRE / JVM) that’s built into Tomcat and provides an environment in which Servlets can be run.
		
		
	52. What is mod_vhost_alias?
	Answer: It allows hosting multiple sites on the same server via simpler configurations.

			Worker MPM uses multiple child processes with many threads each. Each thread handles one connection at a time.
			Prefork MPM uses multiple child processes with one thread each and each process handles one connection at a time.





























	
	
	


	





