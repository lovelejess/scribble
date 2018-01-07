---
title: bash commands search & displaying
date: 2015-10-18 011:49:21
---

##How to display all processes currently running

`ps aux` 

####Let's break it down or visit [explain shell.com](http://explainshell.com/explain?cmd=ps+aux)

`ps` -> list snapshot of the current processes

`a` -> list all processes

`u` -> including all other users

`x` -> excluding processes that are TTYs (anything that is a psuedo terminal)



##How to search for a pattern that occurs in the current directory and its sub directories

`grep -nr searchPattern .`

####Let's break it down or visit [explain shell.com](http://explainshell.com/explain?cmd=grep+-nr+.)

`grep` -> prints lines matching a pattern

`-n` -> flag to print the line number of the file in which the pattern was found

`-r` -> read all files in its subdirectory, recursively

`searchPattern` -> your pattern you are trying to find

`.` -> this current directory