# How to Check Apache Concurrent Connections in Linux Using netstat and ss Command?

by Shamsher Kushwaha · Last Updated: Decmeber 11, 2020

    As a Linux Server Administrator we need to learn all kind of troubleshooting skill & method, So that we can handle the situation without panic when issue is occurs.

    If server load is high, we use top command or htop command to check the server performance at first.

    This is the first troubleshooting step that every Linux admin use.

        How to Understand Linux Top Command Output and Usage?

    The server is primarily used as a web server and top/htop commands are showing many Apache requests coming from different resources then what would you do?

    Apache is one of the famous web services, which is serving more than 100 Million website.

    In this situation, you have to check the Apache (httpd) current connections

    The server might be under stress or it could be under attack, especially it could be DDOS attack.

    When the server is overloaded, you may want to check how many connections are active in Apache and which IP is taking maximum number of connection from Apache.

    There are many ways that we can check this. But i would like to check this using netstat and ss command.

    These commands are widly used by system administrators and security professionals to check the network connections in Linux servers.
    What’s ss Command?

    ss stands for socket statistics. It is used to dump socket statistics about network/socket connections.

    It’s showing information similar to netstat, it works better and faster compared with netstat. It can display more TCP and state information than other tools.

    Since the ss command gets all the information directly from kernel space (with single source) that’s why it’s faster than netstat.
    What’s netstat Command?

    netstat stands for network statistics. It displays network connections, routing tables, interface statistics, masquerade connections, multicast memberships and network protocol statistics.

    The netstat command has been deprecated and replaced by the ss command in most of the Linux distributions.

    It reads various /proc files to gather information. It would take more time when there are lots of connections to display.
    How to Count Apache’s (Httpd) Current Connections in Linux Using ss Command?

    Use the ss command with following options to count Apache current connections in Linux.

    $ss -ant | grep :80 | wc -l

        110

### How to Count Apache’s (Httpd) Current Connections in Linux Using netstat Command?

    Use the netstat command with the following options to count apache current connections in Linux.

    # netstat -ant | grep :80 | wc -l
    90

### How to Display Apache’s (Httpd) Current Connections in Linux Using ss Command?'



    Use the ss command with the following options to display detailed information about apache’s current connections in Linux.

    It displays an active internet connections in the server at port 80.

    # ss -ant | grep :80
    LISTEN     0      128          *:80                       *:*                  
    TIME-WAIT  0      0      94.237.64.70:80                 172.69.44.142:16690              
    TIME-WAIT  0      0      94.237.64.70:80                 172.69.68.173:60360              
    TIME-WAIT  0      0      94.237.64.70:80                 172.68.146.130:25988              
    TIME-WAIT  0      0      94.237.64.70:80                 162.158.231.34:52566              
    TIME-WAIT  0      0      94.237.64.70:80                 162.158.231.34:54094              
    TIME-WAIT  0      0      94.237.64.70:80                 108.162.229.40:21418              
    TIME-WAIT  0      0      94.237.64.70:80                 162.158.231.145:33218              
    TIME-WAIT  0      0      94.237.64.70:80                 162.158.50.236:63306              
    TIME-WAIT  0      0      127.0.0.1:56262              127.0.0.1:80                 
    ESTAB      0      0      94.237.64.70:80                 172.68.169.24:35698              
    TIME-WAIT  0      0      94.237.64.70:80                 172.68.58.127:59962              
    TIME-WAIT  0      0      94.237.64.70:80                 162.158.146.186:23750              
    TIME-WAIT  0      0      94.237.64.70:80                 162.158.231.31:20908              
    TIME-WAIT  0      0      94.237.64.70:80                 162.158.231.7:17936              
    TIME-WAIT  0      0      94.237.64.70:80                 162.158.231.13:33318              
    TIME-WAIT  0      0      94.237.64.70:80                 162.158.231.22:23296              
    ESTAB      0      0      94.237.64.70:80                 162.158.38.72:45150          

###  to Display Apache’s (Httpd) Current Connections in Linux Using netstat Command?

    Use the netstat command with the following options to display detailed information about apache’s current connections in Linux.

    It displays an active internet connections in the server at port 80.

    # netstat -ant | grep :80
    tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN     
    tcp        0      0 94.237.64.70:80         172.69.44.142:16690     TIME_WAIT  
    tcp        0      0 94.237.64.70:80         172.69.68.173:60360     TIME_WAIT  
    tcp        0      0 94.237.64.70:80         172.68.146.130:25988    TIME_WAIT  
    tcp        0      0 94.237.64.70:80         162.158.231.34:52566    TIME_WAIT  
    tcp        0      0 94.237.64.70:80         162.158.231.145:33218   TIME_WAIT  
    tcp        0      0 94.237.64.70:80         162.158.50.236:63306    TIME_WAIT  
    tcp        0      0 127.0.0.1:56262         127.0.0.1:80            TIME_WAIT  
    tcp        0      0 94.237.64.70:80         172.69.68.6:15564       TIME_WAIT  
    tcp        0      0 94.237.64.70:80         172.69.69.240:58040     TIME_WAIT  
    tcp        0      0 94.237.64.70:80         162.158.78.150:51818    ESTABLISHED
    tcp        0      0 94.237.64.70:80         162.158.231.27:56412    TIME_WAIT  
    tcp        0      0 94.237.64.70:80         172.69.70.187:55842     TIME_WAIT  
    tcp        0      0 94.237.64.70:80         141.101.76.126:58756    ESTABLISHED
    tcp        0      0 94.237.64.70:80         172.68.50.30:16508      ESTABLISHED
    tcp        0      0 94.237.64.70:80         162.158.231.7:17780     TIME_WAIT  
    tcp        0      0 94.237.64.70:80         162.158.231.16:60012    TIME_WAIT  
    tcp        0      0 94.237.64.70:80         162.158.231.24:13312    TIME_WAIT  
    tcp        0      0 94.237.64.70:80         162.158.231.13:30752    TIME_WAIT  
    tcp        0      0 94.237.64.70:80         162.158.154.93:42576    ESTABLISHED

### How to Count Number of Connection Currently Active in Apache from Each IP Address Using ss Command?

    Use the ss command with the following options to count number of connection currently active in Apache from each IP address.

    # netstat -ant | awk '{print $5}' | cut -d":" -f1 | sort | uniq -c | sort -nr
        6 162.158.155.70
        5 127.0.0.1
        2 172.68.51.180
        2 172.68.215.98
        2 172.68.215.86
        2 172.68.215.77
        2 172.68.215.75
        2 172.68.215.113
        2 172.68.215.111
        2 172.68.215.109
        2 172.68.215.101
        2 172.68.215.100
        2 162.158.150.128
        2 162.158.150.120
        2 162.158.118.154
        2 141.101.96.253
        2 141.101.96.243
        2 141.101.76.234
        2 141.101.105.254
        .
        .

### How to Count Number of Connection Currently Active in Apache from Each IP Address Using netstat Command?

    Use the netstat command with the following options to count number of connection currently active in Apache from each IP address.

    # ss -at | awk '{print $5}' | cut -d":" -f1 | sort | uniq -c | sort -nr
        6 162.158.155.70
        5 127.0.0.1
        2 172.68.51.180
        2 172.68.215.98
        2 172.68.215.86
        2 172.68.215.77
        2 172.68.215.75
        2 172.68.215.113
        2 172.68.215.111
        2 172.68.215.109
        2 172.68.215.101
        2 172.68.215.100
        2 172.68.169.36
        2 162.158.150.128
        2 162.158.150.120
        2 162.158.118.154
        2 141.101.96.253
        2 141.101.96.243
        2 141.101.76.234
        2 141.101.105.254
        .
        .

### How to Count Number of Connection Currently Established in Apache from Each IP Address Using ss Command?

    Use the ss command with the following options to count number of connection currently established in Apache from each IP address.

    # ss -at | grep ESTAB | awk '{print $5}' | cut -d":" -f1 | sort | uniq -c | sort -n
        1 103.5.134.182
        1 162.158.150.84
        1 172.68.169.30
        1 172.68.206.84
        1 182.111.155.129
        1 69.10.49.214

### How to Count Number of Connection Currently Established in Apache from Each IP Address Using netstat Command?

    Use the netstat command with the following options to count number of connection currently established in Apache from each IP address.

    # netstat -ant | grep ESTAB | awk '{print $5}' | cut -d":" -f1 | sort | uniq -c | sort -n
        1 103.5.134.182
        1 172.68.206.84
        1 182.111.155.129
        1 69.10.49.214

### How to Count Apache’s (httpd) Actual Running Processes in Linux Using ps Command?

    ps command is used to display all running processes in Linux system. Use the following format, if you would like to count running Apache processes in Linux.

    # ps -auxw | grep httpd | grep -v grep | wc -l
    12

### How to Check List of Apache Processes in Linux Using ps Command?

    ps command is used to display all running processes in Linux system. Use the following format, if you would like to display running httpd processes in Linux.

    # ps auxw | grep httpd | grep -v grep
    nobody    7988  0.0  0.5 253280 23252 ?        S    14:32   0:00 /usr/sbin/httpd -k start
    nobody    8050  0.0  0.6 253412 24276 ?        S    14:33   0:00 /usr/sbin/httpd -k start
    nobody    8054  0.0  0.6 253280 23288 ?        S    14:33   0:00 /usr/sbin/httpd -k start
    nobody    8158  0.0  0.6 253280 23296 ?        S    14:33   0:00 /usr/sbin/httpd -k start
    nobody    8159  0.0  0.5 253280 23176 ?        S    14:33   0:00 /usr/sbin/httpd -k start
    daygeek   8202  0.0  0.6 253416 23304 ?        S    14:34   0:00 /usr/sbin/httpd -k start
    nobody    8203  0.0  0.5 253280 23052 ?        S    14:34   0:00 /usr/sbin/httpd -k start
    nobody    8207  0.0  0.5 253280 23044 ?        S    14:34   0:00 /usr/sbin/httpd -k start
    nobody    8213  0.0  0.6 253280 23300 ?        S    14:34   0:00 /usr/sbin/httpd -k start
    nobody    8216  0.0  0.5 253280 23052 ?        S    14:34   0:00 /usr/sbin/httpd -k start
    nobody    8218  0.0  0.6 253416 23304 ?        S    14:34   0:00 /usr/sbin/httpd -k start
    nobody    8266  0.0  0.5 253148 23052 ?        S    14:35   0:00 /usr/sbin/httpd -k start
    nobody    8267  0.0  0.5 253144 22800 ?        S    14:35   0:00 /usr/sbin/httpd -k start
    nobody    8391  0.3  0.5 253144 22800 ?        S    14:35   0:00 /usr/sbin/httpd -k start
    nobody    8393  0.5  0.5 253012 21776 ?        S    14:35   0:00 /usr/sbin/httpd -k start
    nobody    8394  1.0  0.5 253144 22800 ?        S    14:35   0:00 /usr/sbin/httpd -k start
    root     30500  0.0  0.0 227356  3584 ?        Ss   Jul25   2:33 /usr/sbin/httpd -k start
