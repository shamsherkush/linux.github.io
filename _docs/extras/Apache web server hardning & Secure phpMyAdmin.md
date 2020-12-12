### Ports- 

    WHM - 2086
    WHM-SSL- 2087
    cPanel- 2082
    cPanel-SSL- 2083 
    Webmail	- 2095
    Webmail-SSL	2096


    POP3			110
    POP3 - SSL		995
    IMAP			143
    IMAP - SSL		993
    SMTP			25
    SMTP - SSL		465

    20 – FTP Data (For transferring FTP data)

    21 – FTP Control (For starting FTP connection)
    23 – Telnet (For insecure remote administration)
    25 – SMTP (Mail Transfer Agent for e-mail server such as SEND mail)
    68 – DHCP
    69 – TFTP 

    123 – NTP
    139 – SMB-Samba
    143 – IMAP
    161 – SNMP (For network monitoring)
    389 – LDAP (For centralized administration)
    514 – Syslogd (udp port)
    873 – rsync

    1194 – openVPN



### Difference between Apache and Nginx
    1= Apache is a open-source HTTP server whereas Nginx is a high-performance asynchronous web server and reverse proxy server.

    2= While Apache provides a variety of multiprocessing modules to handle client requests and web traffic,
        Nginx is so designed to handle multiple client requests simultaneously with minimal hardware resources.

    3= In Apache HTTP server,single thread is associated with only one connection, whereas a single thread in Nginx can handle
        multiple connections.All the processes are put in an event loop along with other  connections and are managed asynchronously.

    4= It handels dynamic content within the web server itself. Nginx cannot process dynamic content natively.

    5= Modules are dynamically loaded or unloaded making it more flexible. IN Nginx Modules cannot be loaded dynamically.They must be compiled
        within the core sofwate itself.

    6= Apache is designed to be a web server. Nginx is both a web server and proxy server.

    7= A singal thread can only process one connection. but A singal thread can handel multiple connections.


### What is the difference between application server and web server?

    1= Web Server is designed to serve HTTP Content. Application Server can also serve HTTP Content but is not 
        limited to just HTTP. It can be provided other protocol support such as RMI/RPC

    2= Web Server is mostly designed to serve static content, though most Web Servers have plugins to support scripting languages like Perl, 
        PHP, ASP, JSP etc.through which these servers can generate dynamic HTTP content.

    3= Web servers are suitable for static content. Application servers are suitable for dynamic content.

    4= Involve only web or servlet container and cannot be used for EJB.but Could contain a web server as an aggregate part of them and also contain web and EJB containers.

    5= WEB SERVER Not supported Multithreading but APPLICATION SERVER Supports multithreading
    6= WEB SERVER Use HTML and HTTP but APPLICATION SERVER Use Graphical user interface, HTTP, RPC/RMI.

    7= WEB SERVER Provides environment to run Web application but APPLICATION SERVER Provides environment to run Enterprise application.

### Error Codes

        200 successful – the request was successfully received, understood, and accepted

        300 redirection error – further action needs to be taken in order to complete the request
        
        400 client error – the request contains bad syntax or cannot be fulfilled

        500 server error – the server failed to fulfil an apparently valid request

        501 Not Implemented - The server either does not recognize the request method, or it lacks the ability to fulfil the request.

        502 Bad Gateway    -   The server was acting as a gateway or proxy and received an invalid response from the upstream server.

        503 Service Unavailable - The server cannot handle the request (because it is overloaded or down for maintenance).

        504 Gateway Timeout -  The server was acting as a gateway or proxy and did not receive a timely response from the upstream server.

        505 HTTP Version Not Supported

        400 Bad Request = It means that the request could not be understood by the server due to malformed syntax. 

        401 Unauthorized = This status code comes when the request requires user authentication.

        403 Forbidden = This status code is the one when the user is not authorized to perform the operation or the resource is unavailable for some reason.

        404 Not Found = This the error code stands for when the requested webpage is not found on the specified server.

        500 Internal Server Error = The 500 are server errors comes when the server encounters an unexpected condition which prevented it from fulfilling the request.


### How to Configure Basic Apache Reverse Proxy--------How to Configure Basic Apache Reverse Proxy

    Benefits of a reverse proxy   (Load balancing, Central logging, Improved security, Better performance)

    Listen 80
    Listen 8080

    <VirtualHost 172.20.30.40:80>


    Mixed port-based and ip-based virtual hosts

    Listen 172.20.30.40:80
    Listen 172.20.30.40:8080

    <VirtualHost 172.20.30.40:80>
    <VirtualHost 172.20.30.40:8080>



    <VirtualHost *:80>
        ServerAdmin root@rozgar.com
        ServerName api.rozgar.com
        ServerAlias api.rozgar.com
        DocumentRoot /var/www/webserver/apirozgar/
    <Directory "/var/www/webserver/apirozgar/">
            AllowOverride All
            Options FollowSymLinks
            Order Allow,Deny
            Allow from all
    </Directory>

    ProxyRequests Off											(FOR HTTPD)
    ProxyPass / http://0.0.0.0:8080/
    ProxyPassReverse / http://localhost:8080/


    or 
        location /some/path/ {												(FOR Nginx)
            proxy_pass http://127.0.0.1:8000;

    CustomLog "|/usr/sbin/rotatelogs /var/www/httpdlog/apirozgar/access.%Y-%m-%d.log 86400" combined
    ErrorLog "|/usr/sbin/rotatelogs /var/www/httpdlog/apirozgar/error.%Y-%m-%d.log 86400"
    </VirtualHost>




### Set all directories to 755, and all files to 644 (VERY VERY IMPORTANT PART )

        If you need a quick way to reset your public_html data to 755 for directories and 644 for files, then you can use something like this:
        ##cd /home/user/domains/domain.com/public_html
        ##find . -type d -exec chmod 0755 {} \;
        ##find . -type f -exec chmod 0644 {} \;

        additionally, if you know php runs as the user and not as "apache", then you can set php files to 600, for an extra level of security, eg:
        ##find . -type f -name '*.php' -exec chmod 600 {} \;
        ##find . -type f -name '*.php' -exec chmod 600 {} \;


## Apache Logs

### HOW MANY HITS ON YOUR APACHE Server WITH DATE 

    cut -d'[' -f2 /var/log/httpd/access_log | cut -d':' -f1-3 |uniq -c

### HOW MANY Current USER ON YOUR APACHE Server

    netstat -anp | grep ":80" | grep ESTABLISHED | wc -l

    Or if you want total number of hits, take the uniq out:
    cat /var/log/httpd/access_log | cut -d" " -f1 | sort -n | uniq | wc -l
    cat /var/log/httpd/access_log | cut -d" " -f1 | sort -n | wc -l


    WITH IPAddress Take the wc -l off at the end like this to get all the IPAddresses that have visited your site:
    #cat /var/log/httpd/access_log | cut -d" " -f1 | sort -n | uniq       (get ALL IPAddress)

    #ps -ef | grep apache | grep -v gep | wc -l         (to Know connection number)   

### Apache Web Server Security and Hardening Tips

    1. Hide Apache Version and OS Identity from Errors
        ServerSignature Off
        ServerTokens Prod

    2. Disable Directory Listing       & Block Directory Access  
        <Directory /var/www/html>
        Options -Indexes				Require all denied
        </Directory>

    3. Keep updating Apache Regularly
        # httpd -v
        # yum update httpd
        
    3. Secure Apache With HTTPD trace & File Etaging 
        FileETag None       (ETag header)
        TraceEnable off
        Timeout 60
        expose_php = Off

    4. Apache Twiks Enable mod_mpm_worker  
        Worker MPM uses multiple child processes with many threads each. Each thread handles one connection at a time.
        Prefork MPM uses multiple child processes with one thread each and each process handles one connection at a time.
        vim /etc/httpd/conf.modules.d/00-mpm.conf    (Enable MPM Modual)
        <IfModule mpm_worker_module>   
        ServerLimit         	250
        #MaxClients 				512
        StartServers        	10
        MinSpareThreads     	75
        MaxSpareThreads     	250
        ThreadLimit         	10000
        ThreadsPerChild     	1500
        MaxRequestWorkers   	9000
        MaxConnectionsPerChild  500
        </IfModule>
        
        
    5. Limit Request Size    (total size of the HTTP request) (you want to limit the upload size for a particular directory)
        <Directory "/var/www/myweb1/user_uploads">
        LimitRequestBody 512000
        </Directory>


    4. Disable Unnecessary Modules   									(vim /etc/httpd/conf/httpd.conf)
        mod_imap, mod_include, mod_info, mod_userdir, mod_autoindex.

    5. Run Apache as separate User and Group
        # User apache
        # groupadd apache
        # useradd -d /var/www/ -g apache -s /bin/nologin apache

    6. Use Allow and Deny to Restrict access to Directories
        <Directory />
        Options None
        Order deny,allow
        Deny from all
        </Directory>

    7. Use mod_security and mod_evasive Modules to Secure Apache
        Mod_security = Where mod_security works as a firewall for our web applications and allows us to monitor traffic on a real time basis.
        mod_evasive  = Mod_evasive is the module that will protect your Apache server from brute force and DDoS attacks, 
        # yum install mod_security    #  LoadModule security2_module modules/mod_security2.so

    8. Disable Apache’s following of Symbolic Links
        # Enable symbolic links
        Options +FollowSymLinks

    9. Turn off Server Side Includes and CGI Execution
        We can turn off server side includes (mod_include) and CGI execution if not needed and to do so we need to modify main configuration file.
        Options -Includes
        Options -ExecCGI

        <Directory "/var/www/html/web1">
        Options -Includes -ExecCGI
        </Directory>

    10. Limit Request Size    (total size of the HTTP request) (you want to limit the upload size for a particular directory)
        <Directory "/var/www/myweb1/user_uploads">
        LimitRequestBody 512000
        </Directory>

    11. Protect DDOS attacks and Hardening
        TimeOut :300 secs
        MaxClients :256.
        KeepAliveTimeout :  5 secs
        LimitRequestFields : 100


    12. Enable Apache Logging
        <VirtualHost *:80>
        DocumentRoot /var/www/html/example.com/
        ServerName www.example.com
        DirectoryIndex index.htm index.html index.php
        ServerAlias example.com
        ErrorDocument 404 /story.php
        ErrorLog /var/log/httpd/example.com_error_log
        CustomLog /var/log/httpd/example.com_access_log combined
        </VirtualHost>

    13. Securing Apache with SSL Certificates
        # openssl genrsa -des3 -out example.com.key 1024
        # openssl req -new -key example.com.key -out exmaple.csr
        # openssl x509 -req -days 365 -in example.com.com.csr -signkey example.com.com.key -out example.com.com.crt

        <VirtualHost 172.16.25.125:443>
        SSLEngine on
        SSLCertificateFile /etc/pki/tls/certs/example.com.crt
        SSLCertificateKeyFile /etc/pki/tls/certs/example.com.key
        SSLCertificateChainFile /etc/pki/tls/certs/sf_bundle.crt
        ServerAdmin ravi.saive@example.com
        ServerName example.com
        DocumentRoot /var/www/html/example/
        ErrorLog /var/log/httpd/example.com-error_log
        CustomLog /var/log/httpd/example.com-access_log common
        </VirtualHost>



    14 	Increase PHP Perforance (Tuning PHP)
        vim /etc/php.ini
            short_open_tag = On
            post_max_size = 500M  
            output_buffering = 4096
            max_execution_time = 600
            max_input_time = 120
            max_input_vars = 5000
            memory_limit = 512M
            upload_max_filesize = 200M
            session.gc_maxlifetime = 7200         (Increase apache session time from PHP)
            query_cache_limit = 16M
            query_cache_size = 48M
            key_buffer = 256M
            suhosin.get.max_vars = 10000
            suhosin.post.max_vars = 10000
            
    15. TSL (Tranport Layer Secure) and SSL (Secure socket layer)
	
	

##  How to install and secure phpMyAdmin on Centos 7 --

### How to install and secure phpMyAdmin on Centos 7    (  Securing the phpMyAdmin  )

    1. Disable root Login
        #vim /etc/phpMyAdmin/config.inc.php
        $cfg['Servers'][$i]['AllowRoot'] = TRUE;      Replace To  $cfg['Servers'][$i]['AllowRoot'] = FALSE;

    2. Change the Alias
        #vim /etc/httpd/conf.d/phpMyAdmin.conf
        Alias /myownalias /usr/share/phpMyAdmin
        
    3. Protect with HTTP authentication
        #yum install httpd-tools
        htpasswd -c /etc/phpMyAdmin/.htpasswd linkmount    =    Mount@Link@12345%
        
        #vim /etc/httpd/conf.d/phpMyAdmin.conf
        Add the red part within the “/usr/share/phpMyAdmin” directive like below:
        <Directory /usr/share/phpMyAdmin/>
        AllowOverride All
        <IfModule mod_authz_core.c>
        . . .
        </Directory>
        
        #vim /usr/share/phpMyAdmin/.htaccess
        AuthType Basic
        AuthName "Admin Login"
        AuthUserFile /etc/phpMyAdmin/.htpasswd
        Require valid-user
        
        
        #systemctl reload httpd



### UBUNTU SERVER 

    Enable apache mod_rewrite in Ubuntu 14.04 LTS

    Activate the mod_rewrite module with
    sudo a2enmod rewrite

    sudo service apache2 restart

    sudo vim .htaccess  
    RewriteEngine on


    500 internal server error. to ==>> This error means there is a problem on the server side.
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    vim /etc/apache2/sites-available/000-default. conf    	(VVIP)


    #a2ensite sampledomain.com.conf      					(Enable the domain configuration file)
    #a2dissite 000-default.conf								(disable the domain configuration file)
    #a2enconf servername
    #a2enmod name_of_module		=	To enable the module:
    #a2dismod name_of_module	=	To disable the module:


    #systemctl restart apache2
    #apache2ctl -t   or  #apache2ctl configtest
    #apache2ctl -S
    #apache2 -l
    #/etc/apache2/apache2.conf
    #/etc/apache2/sites-available/000-default. conf
    #/etc/apache2/sites-enabled/000-default.conf
    #/etc/apache2/apache2.conf

    #sites-available/: Storage for Apache virtual host files. A virtual host is a record of one of the websites hosted on the server.
    #sites-enabled/: This directory holds websites that are ready to serve clients.  #a2ensite sampledomain.com.conf


    #Global Configuration Section  
        Timeout , KeepAlive , MaxKeepAliveRequests , KeepAliveTimeout , MPM Configuration


## Some QA--

    Q How to know if a web server is running
    An = ps -ef |grep httpd , netstat -anlp |grep 80

    Q How to ensure Apache listen on only one IP address on the server?
    An = Listen 10.10.10.10:80

    Q How do I disable directory indexing
    An = <Directory />
        Options -Indexes
        </Directory>
        
    Q Which module is required to have redirection possible
    An = mod_rewrite is responsible for the redirection,and this must be uncommented in httpd.conf file.
        LoadModule rewrite_module modules/mod_rewrite.so
        
    Q How to secure Website hosted on Apache Web Server
    An = Implementing SSL , Integrating with WAF (Web Application Firewall) , 

    Q How to create a CSR, 
    An = openssl req -out geekflare.csr -newkey rsa:2048 -nodes -keyout geekflare.key

    Q What module is needed to connect to WebSphere
    An = mod_was_ap22_http.so must be added in httpd.conf file to integrate with IBM WAS.

    Q = How to put Log level in Debug mode?
    An = vim httpd.conf   = LogLevel debug

    Q Which module is required to enable SSL?
    An= LoadModule auth_basic_module modules/mod_ssl.so

    Q What’s the WebLogic module name
    An =mod_wl_22.so

    Q What is the log level available in Apache
    An = debug , info , warn, notice, crit, alarm, emerg, error.

    Q Apache Web Server and Apache Tomcat?
    An = Apache Web is HTTP server to serve static contents where Tomcat is servlet container to deploy JSP files.
        
    Q How can Apache act a Proxy Server
    An = You can use a mod_proxy module to use as a proxy server.
        The mod_proxy module can be used to connect to the backend server like Tomcat,WebLogic, WebSphere, etc.
        
    Q How to configure Apache log, so it captures time taken to serve a request?
    An = LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %D" combined

    Q What tool do you use for log analysis
    An =  GoAccess , SumoLogic , or few mentioned here
            URL Must See = https://geekflare.com/monitor-analyze-access-logs-goaccess/
        

    Q How to verify httpd.conf file to ensure no configuration syntax error?
    An = httpd -t

    Q How to perform Apache performance benchmark
    An = You can use a tool like ApacheBench, SIEGE to perform the load test on web servers including Apache.
        1> ApacheBench  = yum install httpd-tools = # ab -n 5000 -c 500 http://localhost:80/
        2> Nginx = # ab -n 5000 -c 500 http://localhost:80/
        3> SIEGE = # yum install siege  = # siege -q -t 5S -c 500 http://localhost/
        [-q – to run it quietly (not showing request details), -t – run for 5 seconds, -c – 500 concurrent requests]

    Q How to ensure web server is getting started after server reboot?
    An = chkconfig apache on

    Q difference between Apache and Nginx web server?
    An = Nginx is event-based web server where Apache is process based
        Nginx is known for better performance than Apache
        Apache supports wide range of OS where Nginx doesn’t support OpenVMS and IBMi
        Apache has large number of modules integration with backend application server where Nginx is still catching up
        Nginx is lightweight and capturing the market share rapidly.
        
    Q How to hide server version details in HTTP response header
    An = ServerTokens Prod
        ServerSignature Off

    Q What does 200, 403 & 503 HTTP error code mean
    An = 200 – content found and served OK
        403 – tried to access restricted file/folder
        503 – the server is too busy to serve the request and in another word – service unavailable.
        400 - Bad Request
        401 - Not Authuorised
        403 - Forbidden trying access you donot have permission
        500 - Internal server error 
        [Client Error = 400 all ] [Redirection Response = 300 all ] [500 ALL Server error] [Success Response = 200]

    Q How to disable trace HTTP request	 
        An = Add the following in httpd.conf file and restart the instance
            TraceEnable off
        
    Q How do you tell what process has a TCP port open in Linux
    An = netstat -a | grep tcp

    Q Give an example of a set of shell commands that will give you the number of “httpd” processes running on a Linux box.
    An = ps -ef | grep httpd | grep -v grep | wc -l

    Q = How do you execute a UNIX command in the background?
    An = # nohup command-with-options & , command &

    Q What is the main difference between <Location> and <Directory> sections?
    An = Directory sections refer to file system objects; Location sections refer to elements in the address bar of the Web page

    Q: – What is the use of mod_perl module?
    An = mod_perl scripting module to allow better Perl script performance and easy integration with the Web server.

    Q: – Can you record all the cookies sent to your server by clients in Web Server logs?
    An = Yes, add following lines in httpd.conf file.
        CustomLog logs/cookies_in.log "%{UNIQUE_ID}e %{Cookie}i" CustomLog logs/cookies2_in.log "%{UNIQUE_ID}e %{Cookie2}i"
        
    Q: – Can we do automatically roll over the Apache logs at specific times without having to shut down and restart the server?
    An = Yes , Use CustomLog and the rotatelogs programs
        Add following line in httpd.conf file. CustomLog "| /path/to/rotatelogs /path/to/logs/access_log.%Y-%m-%d 86400" combined
        
    Q: – What we can do to find out how people are reaching your site?
    An = Add the following effector to your activity log format. %{Referer}
        
    Q: – How you will put a limit on uploads on your web server?
    An = <Directory "/var/www/html/data_uploads">
        LimitRequestBody 100000
        </Directory>
        
    Q: – I want to stop people using my site by Proxy server. Is it possible?
    An = <Directory proxy:http://www.test.com/myfiles>
        Order Allow,Deny
        Deny from all
        Satisfy All
        </Directory>
        
    Q: – What is mod_evasive module?
    An = mod_evasive is a third-party module that performs one simple task, and performs it very well.
        It detects when your site is receiving a Denial of Service (DoS) attack,
        and it prevents that attack from doing as much damage. mod_evasive detects when a single client is making 
        multiple requests in a short period of time, and denies further requests from that client.

    Q: -Which of the following HTTP headers will direct a browser to www.nextstep4it.com after waiting five seconds?
    An = Reload https://www.nextstep4it.com/ -t 5

    Q = On which port Apache listens http and https both?
    An = # netstat -antp | grep http

    Q  What do you understand by “DirectoryIndex”?
    An =  DirectoryIndex is the name of first file which Apache looks for when a request comes from a domain. 

    Q  What’s the difference between <Location> and <Directory>?
    An = <Location> is used to set element related to the URL / address bar of the web server.
        <Directory> refers that the location of file system object on the server
        
    Q What is mod_perl and mod _php?
    An = mod_perl is an Apache module which is compiled with Apache for easy integration and to increase the performance of Perl scripts.
        mod_php is used for easy integration of PHP scripts by the web server, it embeds the PHP interpreter inside the Apache process.
        Its forces Apache child process to use more memory and works with Apache only but still very popular.

    Q How SSL works with Apache
    An = Apache generates its private key and converts that private key to .CSR file (Certificate signing request).
        Then Apache sends the .csr file to the CA (Certificate Authority).
        CA will take the .csr file and convert it to .crt (certificate) and will send that .crt file 
        back to Apache to secure and complete the https connection request.
        
    Q: - What do you mean by a valid ServerName directive?
    An = The DNS system is used to associate IP addresses with domain names. The value of ServerName is returned whenthe server generates a URL.
        If you are using a certain domain name, you must make sure that it is included in your DNSsystem and will be available to clients 
        visiting your site.
	 

