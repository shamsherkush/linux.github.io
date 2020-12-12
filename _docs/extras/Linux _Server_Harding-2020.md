---
title:  "Linux _Server_Harding-2020"
subtitle: "It's always a bit messy"
author: "Shamsher Kushwaha"
avatar: "img/authors/43068991.png"
image: "img/apache-security-hardening-guide.png"
date:   2020-11-29 20:51:12
---

### 1. Physical System Security

    Configure the BIOS to disable booting from CD/DVD, External Devices, Floppy Drive in BIOS. Next, enable BIOS password & also protect GRUB with password to restrict physical access of your system.
    How to Password Protect GRUB
    [root@tecmint ~]#  grub-md5-crypt
    $1$19oD/1$NklcucLPshZVoo5LvUYEp1

    [root@shamsher ~]# vi /boot/grub/grub.conf
    password --md5 $1$TNUb/1$TwroGJn4eCd4xsYeGiBYq.

### 2. Disk Partitions
    /
    /boot
    /usr
    /var
    /home
    /tmp
    /opt

### 3. Minimize Packages to Minimize Vulnerability

### 4. Check Listening Network Ports

    # netstat -tulpn
    # netstat -a | more      (Listing all the LISTENING Ports of TCP and UDP connections)
    # netstat -at                (Listing TCP Ports connections)

### 5. Disable Root Login

    # vi /etc/ssh/sshd_config
    PermitRootLogin no
    AllowUsers username

### 6. SSH Server Security

    public key based login
    Disable root user login			= PermitRootLogin no
    Disable password based login    = PasswordAuthentication no
    Limit Users’ ssh access			= AllowUsers vivek jerry
    Disable Empty Passwords			= PermitEmptyPasswords no
    Change SSH Port and limit IP binding = ListenAddress 192.168.1.5


### 7. Keep System updated

    # yum check-update

###  8. Lockdown Cronjobs
    # echo ALL >>/etc/cron.deny

### 9. Disable USB stick to Detect

    install usb-storage /bin/true

### 10. Turn Off IPv6

    # vi /etc/sysconfig/network
    NETWORKING_IPV6=no
    IPV6INIT=no


### 11. Restrict Users to Use Old Passwords

    vi /etc/pam.d/system-auth
    auth        sufficient    pam_unix.so likeauth nullok
    password   sufficient    pam_unix.so nullok use_authtok md5 shadow remember=5

### 12. How to Check Password Expiration of User

    #chage -l username
    #chage -M 60 username
    #chage -M 60 -m 7 -W 7 userName
    -M Set maximum number of days
    -m Set minimum number of days
    -W Set the number of days of warning

### 13. Lock and Unlock Account Manually

    # passwd -l accountName

### 14. Enforcing Stronger Passwords

    # vi /etc/pam.d/system-auth
    /lib/security/$ISA/pam_cracklib.so retry=3 minlen=8 lcredit=-1 ucredit=-2 dcredit=-2 ocredit=-1


### 15. Enable Iptables (Firewalld)  & TCPWrappers 


### 16. Disable Ctrl+Alt+Delete in Inittab
    vim /etc/inittab
### # Trap CTRL-ALT-DELETE
    #ca::ctrlaltdel:/sbin/shutdown -t3 -r now


### 17. Remove KDE/GNOME Desktops
    # yum groupremove "X Window System"

### 18. Checking Accounts for Empty Passwords
    # cat /etc/shadow | awk -F: '($2==""){print $1}'

### 19. Display SSH Banner Before Login
    # vi /etc/issue.net
    # vi /etc/ssh/sshd_config

    #Banner /some/path
    Banner /etc/issue.net


### 20. Review Logs Regularly

    /var/log/message – Where whole system logs or current activity logs are available.
    /var/log/auth.log – Authentication logs.
    /var/log/kern.log – Kernel logs.
    /var/log/cron.log – Crond logs (cron job).
    /var/log/maillog – Mail server logs.
    /var/log/boot.log – System boot log.
    /var/log/mysqld.log – MySQL database server log file.
    /var/log/secure – Authentication log.
    /var/log/utmp or /var/log/wtmp : Login records file.
    /var/log/yum.log: Yum log files.

### 21. Important file Backup

### 22. NIC Bonding
    vim /etc/modprobe.d/bonding.conf
    alias bond0 bonding

    # vi /etc/sysconfig/network-scripts/ifcfg-bond0
    DEVICE=bond0
    IPADDR=192.168.1.8
    NETMASK=255.255.255.0
    ONBOOT=yes
    BOOTPROTO=none
    USERCTL=no

    # vi /etc/sysconfig/network-scripts/ifcfg-eth0
    DEVICE=eth0
    USERCTL=no
    ONBOOT=yes
    MASTER=bond0
    SLAVE=yes
    BOOTPROTO=none

    # vi /etc/sysconfig/network-scripts/ifcfg-eth1
    DEVICE=eth1
    USERCTL=no
    ONBOOT=yes
    MASTER=bond0
    SLAVE=yes
    BOOTPROTO=none

### 23. Keep /boot as read-only
    # vi /etc/fstab
    LABEL=/boot     /boot     ext2     defaults,ro     1 2

### 24. Ignore ICMP or Broadcast Request

    vim /etc/sysctl.conf
    net.ipv4.icmp_echo_ignore_all = 1
    net.ipv4.icmp_echo_ignore_broadcasts = 1
    #sysctl -p

### 25. Use Fail2ban & Clam Antivirus



Securing The Linux Boot Process



