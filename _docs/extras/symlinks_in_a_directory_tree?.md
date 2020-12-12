---
title:  "How do I find all of the symlinks in a directory tree?"
subtitle: "It's always a bit messy"
author: "Shamsher Kushwaha"
avatar: "img/authors/43068991.png"
image: "img/apache-security-hardening-guide.png"
date:   2020-11-29 20:51:12
---

#                                  How do I find all of the symlinks in a directory tree?




#### One command, no pipes

find . -type l -ls

### Explanation: 
find from the current directory . onwards all references of -type link and list -ls those in detail. Plain and simple...

Expanding upon this answer, here are a couple more symbolic link related find commands:

#### Find symbolic links to a specific target
   find . -lname link_target 
   
Note that link_target is a pattern that may contain wildcard characters.

### Find broken symbolic links

find -L . -type l -ls

The -L option instructs find to follow symbolic links, unless when broken.

### Find & replace broken symbolic links

find -L . -type l -delete -exec ln -s new_target {} \;

More find examples

More find examples can be found here: https://hamwaves.com/find/
https://stackoverflow.com/questions/8513133/how-do-i-find-all-of-the-symlinks-in-a-directory-tree
