---
title:  "Linux pgrep command"
subtitle: "It's always a bit messy"
author: "Shamsher Kushwaha"
avatar: "img/authors/43068991.png"
image: "img/apache-security-hardening-guide.png"
date:   2020-11-29 20:51:12
---

#                                         Linux pgrep command     

##############  On Unix-like operating systems, the pgrep command searches for processes currently 
running on the system, based on a complete or partial process name, or other specified attributes.


### Linux pgrep command
The pgrep command in Linux lets users look up processes based on name and other attributes. Following is its syntax:


pgrep looks through the currently running processes and lists the process IDs which match the 
selection criteria to stdout. All the criteria have to match.
Following are some Q&A-styled examples that should give you a good idea of how the pgrep command works.

### Q1. How to find the ID of a process owned by a specific user?

This can be done using the -u command-line option. For example, to find the ID of the 'gedit' process owned by user 'himanshu', use pgrep in the following way:


 
pgrep -u shamsher gedit
Here's the output this command produced in my case:



### Q2. How to make pgrep print count of matching processes?

In case you want the tool to just print the count, and not the IDs themselves, use the -c command-line option. For example, to know the count of processes owned by user 'shamsher,' run the following command:

pgrep -c -u shamsher
Following is the output this command produced on my system:


### Q3. How to use a custom delimiter in output?

By default, the process IDs in output are delimited by a newline. However, if you want, you can change the delimiter, something which you can do using the -d command-line option.

For example, I wanted to use a colon (:) as a delimiter, so I executed the pgrep command in the following way:

pgrep -u shamsher -d:
And following is the output the command produced:

1793:1794:1807:1811:1813:1817:1820:1914:1917:1922:1925:1936:1938:1954:1974:1978:1980:1982:1993:1999
:2008:2009:2012:2020:2024:2034:2036:2043:2048:2049:2051:2052:2055:2064:2068:2073:2074:2085:2088:
2093:2094:2095:2098:2101:2104:2117:2125:2161:2162:2168:2173:2182:2201:2213:2233:2245:2266:2279:
2388:2409:2430:2456:2473:2564:2647:3085:3108:3178:3284:3297:3320:3325:3467:3487:3980:4040:4658:
5668:5721:5777:6271:6281:6512:6808

### Q4. How to make pgrep search case insensitive?

By default, the pgrep search is case sensitive. However, if you want, you can make it case insensitive. For this, you have to use the -i command line option.

For example:

pgrep -i gedit

### Q5. How to make pgrep list process names as well?

For this, use the -l command line option.

Here's an example:

pgrep -u shamsher -l
Here's an excerpt of the output produced on my system:

1793 systemd
1794 (sd-pam)
1807 gnome-keyring-d
1811 gdm-x-session
1813 Xorg
1817 dbus-daemon
1820 gnome-session-b
1914 ssh-agent
1917 gvfsd
1922 gvfsd-fuse
1925 at-spi-bus-laun
1936 dbus-daemon
1938 at-spi2-registr
1954 gnome-shell
1974 ibus-daemon
1978 ibus-dconf
1980 ibus-x11
1982 ibus-portal
1993 gnome-shell-cal
1999 evolution-sourc
2008 dconf-service
2009 gvfs-udisks2-vo
2012 goa-daemon
2020 gvfs-mtp-volume
2024 gvfs-goa-volume
...
...
...
So you can see that in addition to process IDs, process names were also produced in the output.

### Q6. How to make pgrep list full command as well?
In case you want pgrep to display the complete command that was used to launch each process, use the -a command-line option.
Advertisement


pgrep -u shamsher -a

1793 /lib/systemd/systemd --user
1794 (sd-pam)
1807 /usr/bin/gnome-keyring-daemon --daemonize --login
1811 /usr/lib/gdm3/gdm-x-session --run-script env GNOME_SHELL_SESSION_MODE=ubuntu gnome-session --session=ubuntu
1813 /usr/lib/xorg/Xorg vt2 -displayfd 3 -auth /run/user/1000/gdm/Xauthority -background none -noreset -keeptty -verbose 3
1817 /usr/bin/dbus-daemon --session --address=systemd: --nofork --nopidfile --systemd-activation --syslog-only
1820 /usr/lib/gnome-session/gnome-session-binary --session=ubuntu
1914 /usr/bin/ssh-agent /usr/bin/im-launch env GNOME_SHELL_SESSION_MODE=ubuntu gnome-session --session=ubuntu
1917 /usr/lib/gvfs/gvfsd
1922 /usr/lib/gvfs/gvfsd-fuse /run/user/1000/gvfs -f -o big_writes
1925 /usr/lib/at-spi2-core/at-spi-bus-launcher
1936 /usr/bin/dbus-daemon --config-file=/usr/share/defaults/at-spi2/accessibility.conf --nofork --print-address 3
1938 /usr/lib/at-spi2-core/at-spi2-registryd --use-gnome-session
1954 /usr/bin/gnome-shell
1974 ibus-daemon --xim --panel disable
1978 /usr/lib/ibus/ibus-dconf
1980 /usr/lib/ibus/ibus-x11 --kill-daemon
1982 /usr/lib/ibus/ibus-portal
1993 /usr/lib/gnome-shell/gnome-shell-calendar-server
1999 /usr/lib/evolution/evolution-source-registry
2008 /usr/lib/dconf/dconf-service
2009 /usr/lib/gvfs/gvfs-udisks2-volume-monitor
2012 /usr/lib/gnome-online-accounts/goa-daemon
2020 /usr/lib/gvfs/gvfs-mtp-volume-monitor
2024 /usr/lib/gvfs/gvfs-goa-volume-monitor
2034 /usr/lib/gnome-online-accounts/goa-identity-service
2036 /usr/lib/gvfs/gvfs-gphoto2-volume-monitor
2043 /usr/lib/gvfs/gvfs-afc-volume-monitor
Q7. How to make pgrep only display the newest process?
If instead of all processes, you want pgrep to output only the most recent process, then this can be done using the -n command line option.

Here's an example:

pgrep -u shamsher -n -l
And following is the output this command produced:

7163 thunderbird
I can confirm that Thunderbird was indeed the most recent process that was launched by the user 'shamsher.'

### Q8. How to make pgrep only display the oldest process?

For this, use the -o command-line option.

pgrep -u shamsher -o -l
And here's the output this command produced:

1793 systemd

#### Conclusion- 

So you can see that pgrep is an extremely helpful command. Once you are done practicing the command line option we've discussed here, you can head to the tool's man page to learn more about it.
