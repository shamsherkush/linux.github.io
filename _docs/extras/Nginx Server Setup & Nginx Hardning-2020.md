#  										Nginx 

	What is NginX and What are its use cases?
	Web server
	Proxy (Load Balancing, Backend Routing, Caching) (Layer7 & Layer4)

	Nginx is high performance with less resources
	Nginx is basically reverse proxy server but its work as web server also and proxy server For Mail server as well as.
	Nginx works on worker model
	Nginx send rquest to php also 
	Aache works on prefork model

	------------------------------------------------
	#yum install epel-release
	#yum install nginx
	#systemctl start nginx; systemctl enable nginx; systemctl status nginx
	#/usr/share/nginx/html  =  /usr/share/nginx  =  Default Server Root
	#/etc/nginx/nginx.conf.

	-------------------------------------------------------------------------------
	##Install PHP & PHP-FPM on CentOS7
	rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
	rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
	yum install php70w php70w-mysql -y php70w-mcrypt php70w-mbstring

	vim /etc/php.ini
	cgi.fix_pathinfo=0    (Chaneg this line)


	vim /etc/php-fpm.d/www.conf   (listen = 127.0.0.1:9000  replace to listen = /var/run/php-fpm/php-fpm.sock)
	listen.owner = nginx
	listen.group = nginx
	user = nginx
	group = nginx

	systemctl start php-fpm ; systemctl enable php-fpm ; systemctl start php-fpm ; systemctl restart php-fpm ; systemctl restart nginx

	-------------------------------------------------------------------------------
	Before Creating VirtualHost Disable inside Vhost in /etc/nginx/nginx.conf 

	##Configure Nginx Virtual Host
	vim /etc/nginx/conf.d/example.com.conf
	server {
		listen   80;
		server_name  sonu1.sonu1.in;
		root   /sonu;
		index index.php index.html index.htm;
		location / {
			try_files $uri $uri/ =404;
		}
		error_page 404 /404.html;
		error_page 500 502 503 504 /50x.html;
		location = /50x.html {
			root /sonu;
		}
		location ~ \.php$ {
			try_files $uri =404;
			fastcgi_pass unix:/var/run/php-fpm/php-fpm.sock;
			fastcgi_index index.php;
			fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
			include fastcgi_params;
		}
	}

	systemctl reload nginx ; systemctl status nginx


	##Test PHP Processing
	vim usr/share/nginx/html/info.php
	<?php phpinfo(); ?>

	http://example.com/info.php


### how to use reverse proxy nginx+++++++++++++++

	Benefits of Using a Nginx Reverse Proxy on an Instance
	1.) Load Balancing: 
	2.) Increased Security;
	3.) Better Performance: 
	4.) Easy Logging and Auditing:
	5.) Encrypted Connection

	#vim /etc/nginx/sites-available/reverse-proxy.conf

	server {
			listen 80;
			listen [::]:80;
			
			access_log /var/log/nginx/reverse-access.log;
			error_log /var/log/nginx/reverse-error.log;
			
			location / {
						proxy_pass http://127.0.0.1:8000;
						
	}
	}					

### Harden the Nginx Web Server on your CentOS 7 ++++

	1. Hide Nginx Version
	vim /etc/nginx/nginx.conf
		http {
			server_tokens off;

	2. Restrict Access by IP
	vim /etc/nginx/sites-enabled/default
			server {
				location /site-admin/ {
				allow 192.168.1.1/24;
				allow 10.0.0.1/24;
				deny all;
				}

	3. Enable X-XSS Protection     (X-XSS protects the web server against cross-site scripting attacks. )
		http {
			server_tokens off;
			add_header X-XSS-Protection "1; mode=block";
			

	4. Disable Unwanted HTTP Methods    ( provision of stealing cookie information through cross-site tacking attacks.)
	vim /etc/nginx/nginx.conf 
		if ($request_method !~ ^(GET|HEAD|POST)$ )
		{
		return 405;
		}
		
	5. Secure Nginx From A Clickjacking Attack  (Clickjacking attack entails hacker placing a hidden link below legitimate button on site and the user unknowingly clicks)
		vim /etc/nginx/nginx.conf
			add_header X-Frame-Options "SAMEORIGIN";
			

	6. Always keep nginx up to date
		sudo yum update nginx

	7. Set Buffer Size Limitations in Nginx
		To prevent buffer overflow attacks against your Nginx web server, set the following directives in a separate file 
		(create a new file named /etc/nginx/conf.d/buffer.conf, for example):
		client_body_buffer_size  1k;
		client_header_buffer_size 1k;
		client_max_body_size 1k;
		large_client_header_buffers 2 1k;
		
	8. Setup Monitor Logs for Nginx   (nginx access and error logs)
		setup error and access log for saprate virtul host
		
	9. Prevent Image Hotlinking in Nginx
		For example, let’s say you have a subdirectory named img inside your server block where you store all the images used in that 
		virtual host. To prevent other sites from using your images, you will need to insert the following location block inside your 
		virtual host definition:
		
		location /img/ {
			valid_referers none blocked 192.168.0.25;
			if ($invalid_referer) {
				return   403;
			}
		}
		
	10. Use SSL Certificates in Nginx 

	11. Redirect HTTP traffic to HTTPS in Nginx
		return 301 https://$server_name$request_uri;
		
	12. Control simultaneous connections	
		You can use NginxHttpLimitZone module to limit the number of simultaneous connections for the assigned session or as a special case, from one IP address. Edit nginx.conf:
		limit_zone slimits $binary_remote_addr 5m;
		limit_conn slimits 5;

	13. Deny certain User-Agents
		You can easily block user-agents i.e. scanners, bots, and spammers who may be abusing your server.
		
		if ($http_user_agent ~* msnbot|scrapbot) {           (Block robots called msnbot and scrapbot:)
		return 403;
		}
		
	14. Block referral spam  (Referer spam is dangerous. It can harm your SEO ranking via web-logs (if published) as referer field refer to their spammy site. )
		if ( $http_referer ~* (babes|forsale|girl|jewelry|love|nudit|organic|poker|porn|sex|teen) )
		{  
		# return 404;
		return 403;   
		}
		
	15. Directory restrictions
		location /docs/ {
		# block one workstation
		deny    192.168.1.1;

		# allow anyone in 192.168.1.0/24
		allow   192.168.1.0/24;

		# drop rest of the world
		deny    all;
		}

	16. Password Protect The Directory
		mkdir /usr/local/nginx/conf/.htpasswd/
		htpasswd -c /usr/local/nginx/conf/.htpasswd/passwd vivek
		
		vim /etc/nginx/conf
		location ~ /(personal-images/.*|delta/.*) {
		auth_basic  "Restricted"; 
		auth_basic_user_file   /usr/local/nginx/conf/.htpasswd/passwd;
		}
		
		htpasswd -s /usr/local/nginx/conf/.htpasswd/passwd userName
		
	17. Nginx and PHP security tips
		disable_functions = phpinfo, system, mail, exec 		# Disallow dangerous functions 
		max_execution_time = 30
		max_input_time = 60
		memory_limit = 8M
		post_max_size = 8M
		file_uploads = Off
		upload_max_filesize = 2M
		display_errors = Off
		safe_mode = On
		safe_mode_exec_dir = php-required-executables-path    # Only allow access to executables in isolated directory
		safe_mode_allowed_env_vars = PHP_						# Limit external access to PHP environment
		expose_php = Off										# Restrict PHP information leakage
		log_errors = On
		register_globals = Off									# Do not register globals for input data
		post_max_size = 1K
		cgi.force_redirect = 0
		file_uploads = Off
		sql.safe_mode = On
		allow_url_fopen = Off
		
	18. Make use of ModSecurity   = ModSecurity is an open-source module that works as a web application firewall. Different functionalities include filtering, 
		server identity masking, and null byte attack prevention. Real-time traffic monitoring is also allowed through this module. 
		
		

			

	12. (Harding in Linux kernel and networking settings)
	vim /etc/sysctl.conf 
			# Avoid a smurf attack
			net.ipv4.icmp_echo_ignore_broadcasts = 1
	
			# Turn on protection for bad icmp error messages
			net.ipv4.icmp_ignore_bogus_error_responses = 1
	
			# Turn on syncookies for SYN flood attack protection
			net.ipv4.tcp_syncookies = 1
	
			# Turn on and log spoofed, source routed, and redirect packets
			net.ipv4.conf.all.log_martians = 1
			net.ipv4.conf.default.log_martians = 1
	
			# No source routed packets here
			net.ipv4.conf.all.accept_source_route = 0
			net.ipv4.conf.default.accept_source_route = 0
	
			# Turn on reverse path filtering
			net.ipv4.conf.all.rp_filter = 1
			net.ipv4.conf.default.rp_filter = 1
	
			# Make sure no one can alter the routing tables
			net.ipv4.conf.all.accept_redirects = 0
			net.ipv4.conf.default.accept_redirects = 0
			net.ipv4.conf.all.secure_redirects = 0
			net.ipv4.conf.default.secure_redirects = 0
	
			# Don't act as a router
			net.ipv4.ip_forward = 0
			net.ipv4.conf.all.send_redirects = 0
			net.ipv4.conf.default.send_redirects = 0
	
	
			# Turn on execshild
			kernel.exec-shield = 1
			kernel.randomize_va_space = 1
	
			# Tuen IPv6 
			net.ipv6.conf.default.router_solicitations = 0
			net.ipv6.conf.default.accept_ra_rtr_pref = 0
			net.ipv6.conf.default.accept_ra_pinfo = 0
			net.ipv6.conf.default.accept_ra_defrtr = 0
			net.ipv6.conf.default.autoconf = 0
			net.ipv6.conf.default.dad_transmits = 0
			net.ipv6.conf.default.max_addresses = 1
	
			# Optimization for port usefor LBs
			# Increase system file descriptor limit    
			fs.file-max = 65535
	
			# Allow for more PIDs (to reduce rollover problems); may break some programs 32768
			kernel.pid_max = 65536
	
			# Increase system IP port limits
			net.ipv4.ip_local_port_range = 2000 65000
	
			# Increase TCP max buffer size setable using setsockopt()
			net.ipv4.tcp_rmem = 4096 87380 8388608
			net.ipv4.tcp_wmem = 4096 87380 8388608
	
			# Increase Linux auto tuning TCP buffer limits
			# min, default, and max number of bytes to use
			# set max to at least 4MB, or higher if you use very high BDP paths
			# Tcp Windows etc
			net.core.rmem_max = 8388608
			net.core.wmem_max = 8388608
			net.core.netdev_max_backlog = 5000
			net.ipv4.tcp_window_scaling = 1

	

	

		
































