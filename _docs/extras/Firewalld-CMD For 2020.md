---
title:  "How to Manage Firewalld Using firewall-CMD"
subtitle: "It's always a bit messy"
author: "Shamsher Kushwaha"
avatar: "img/authors/43068991.png"
image: "img/apache-security-hardening-guide.png"
date:   2020-11-29 20:51:12
---

## How to Manage Firewalld Using firewall-CMD

	#firewall-cmd --permanent --add-port=22/tcp
	#firewall-cmd --reload
	#firewall-cmd --list-all
	#firewall-cmd --permanent --remove-port=21/tcp


	1. Basic Firewall Command 
		firewall-cmd --state
			
		

	2. Managing Zones	()
		firewall-cmd --get-default-zone  
		firewall-cmd --get-active-zone
		firewall-cmd --list-all-zones
		firewall-cmd --get-zones
		firewall-cmd --permanent --new-zone=test    => firewall-cmd --reload     (Create New Zone)
		firewall-cmd --zone=home --change-interface=eth0						 (Assine interface to zone)
		firewall-cmd --get-zone-of-interface=enp0s3								 (Find which zone is conneted with interface)
		
		
		
		

	3. Source Management		()
		firewall-cmd --permanent --zone=trusted --add-source=192.168.0.1/24			=>			Add Trusted Network
		firewall-cmd --permanent --zone=trusted --remove-source=192.168.0.1/24		=>			Remove Trusted Network
		firewall-cmd --permanent --new-ipset=iplist --type=hash:ip					=>			Create ip-set
		firewall-cmd --ipset=iplist --add-entry=192.168.1.11						=>			Add ip in ip-set
		firewall-cmd --ipset=iplist --add-entry=192.168.1.12
		firewall-cmd --permanent --zone=trusted --add-source=ipset:iplist			=>			Add ip-set for trusted network
		firewall-cmd --zone=internal --add-service=ssh								=>			Assine service for specific ip
		firewall-cmd --permanent --zone=internal --add-service=ssh 
		firewall-cmd --permanent --zone=internal --add-source=1.2.3.4/32 
		firewall-cmd --permanent --zone=public --remove-service=ssh 
		firewall-cmd --permanent --zone=public --add-port=443/tcp		
		firewall-cmd --reload
		firewall-cmd --info-zone=internal											=>			Get details about zone
		
	4. Service Management
	5. Port Management
	6. IP set Management

	7. Masquarading Setting 
		If your firewall is your network gateway and you don’t want everybody to know your internal addresses, you can set up two zones, one called internal, 
		the other external, and configure masquerading on the external zone. This way, all packets will get your firewall ip address as source address.
		
		#firewall-cmd --zone=external --add-masquerade					=>		Add Masqueradeing 
		#firewall-cmd --zone=external –-remove-masquerade
		#firewall-cmd --zone=external -–query-masquerade
		#
		
		
	8. Port Forwarading
		Port forwarding is a way to forward inbound network traffic for a specific port to another internal address or an alternative port.
		Caution: Port forwarding requires masquerading 
		#firewall-cmd --zone=external --add-masquerade
		#firewall-cmd --zone=external --add-forward-port=port=22:proto=tcp:toport=3753     (all packets intended for port 22 to be now forwarded to port tcp 3753 temporarily,)
		#firewall-cmd --permanent --zone=external --add-forward-port=port=22:proto=tcp:toport=3753:toaddr=10.0.0.1
		
	9. How to take backup Firewall Configuration.
		#iptables -S > Firewall-backup-file
		#vim Firewall-backup-file
		#iptables -S > firewalld_rules_ipv4
		#ip6tables -S > firewalld_rules_ipv6
		#vim /etc/firewalld/firewalld.conf
	


### IP Address in Very Easy way-HINDI

	Q:- What is IP Address?
	Ans:- IP address is an unique identification number of a host in a TCP/IP network. It is 32bit binary number, devided into four part.Each Part contain 8 bit.














