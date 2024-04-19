# Linux 
## Why Linux
Linux is an OS open source many distributions
Uses UNIX kernel, it is the core and then the things added on top makes it Linux.
- It is customisable/flexible
- Faster
- Large community
- Stable
- Big knowledge base
- Very employable skill
- Scalable
---
## Need to know commands

`uname` print the name of the OS<br>
`uname` --help this returns a list of commands below<br>

````commandline
  -a, --all                print all information, in the following order,
                             except omit -p and -i if unknown:
  -s, --kernel-name        print the kernel name
  -n, --nodename           print the network node hostname
  -r, --kernel-release     print the kernel release
  -v, --kernel-version     print the kernel version
  -m, --machine            print the machine hardware name
  -p, --processor          print the processor type (non-portable)
  -i, --hardware-platform  print the hardware platform (non-portable)
  -o, --operating-system   print the operating system
      --help     display this help and exit
      --version  output version information and exit
````
`whoami` prints the user 

Bash - born again shell ( is a computer program designed to be run by a Unix shell, a command-line interpreter)<br>
`cat /etc/shells` print list of shells<br>
`ps -p $$` print the current running shell<br>
`ps` - lists current running processes<br>
`-p` specific process<br>
`$$` short form of bash<br>

`history` print the used command history up to 500<br>
`history -c` clears history<br>

`ls` print the list of files<br>
`ls -a` print the list of all files<br>
`ll` print list of files with more information<br>
---
## File and Directory Commands 
`curl` used to transfer data<br>
`curl https://cdn.britannica.com/39/7139-050-A88818BB/Himalayan-chocolate-point.jpg --output cat.jpg`<br>
`file cat.jpg` print the file type<br>

`mv` used to move or rename file<br>
`mv cat.jpg cat`<br>

`cp` used to copy a file<br>
`cp cat cat.jpg`<br>

`rm` to delete a file<br>
`rm cat`<br>

`mkdir` use to make a new directory/folder<br>
`mkdir funny_stuff`<br>
Note: Linux is case-sensitive, space means separate arguments (if you want to use spaces surround the name in "" or \ before space) `mkdir "my pictures"` or `mkdir my\ pictures`<br>
`rm -r` use to delete directories<br>
`rm -rf` use to force delete directories<br>
`rm -r funny`<br>
`rm -rf stuff`<br>

`touch` makes an empty file<br>
`touch chicken-joke.txt`<br>
`nano chicken-joke.txt` to write something in the file<br>
why did the chicken cross the road?<br>
to check out the menu<br>
and catch up with his old friend the colonel<br>
To save in nano: Crtl+X Y Enter<br>

Use a command to print the top 2 lines of chicken-joke.txt to the screen (`head -n 2 chicken-joke.txt`)<br>
Use a command to print the bottom 2 lines of chicken-joke.txt to the screen (`tail -n 2 chicken-joke.txt`)<br>
Use a command to number the lines of chicken-joke.txt when it output the file to the screen (`nl chicken-joke.txt`) (`cat -n chicken-joke.txt`)<br>
Use a command only print to the screen the lines of chicken-joke.txt which contain the keyword chicken (`grep "chicken" chicken-joke.txt`)<br>
---
## Package Commands
`sudo apt install` to install a new package<br>
`sudo apt install tree`<br>
Note: make sure to use `sudo apt update -y` command then install tree
`sudo apt upgrade -y`<br>
Tree lists presenting working directories in a nice way (like a tree)<br>

### HOME and ROOT directories(~)
Linux home directory - /home/ubuntu<br>
Windows home directory - /c/Users/username<br>
`cd` use to take you to home directory<br>
`cd /` use to go to root directory <br>
note: there is nothing before this directory, it is NOT a user! Often times there is a root user (superuser)<br>
`sudo su` use to gain permission that the superuser gets<br>

(Made a directory called funny jokes inside of funny stuff using mkdir and cd)
`mv chicken-joke.txt funny_stuff/`<br>
`mv funny_stuff/chicken-joke.txt .`<br>
note: . move to current folder, be careful not to put / infront as it refers to root directory<br>
`mv chicken-joke.txt funny_stuff/funny_jokes/`<br>
`mv funny_stuff/funny_jokes/chicken-joke.txt funny_stuff/funny_jokes/bad_joke.txt`<br>
note: first argument refers to file you want to modify second argument is the change you want to make<br>
`mv bad_joke.txt ..` moves bad_joke.txt to its parent directory<br>
`rm - r funny_stuff` removes the funny_stuff directory and its contents<br>
---
## Scripting
`nano provision.sh` - set up a server so it is ready to go<br>
`#!/bin/bash` - this is the kind of shell we want to use and where it is in the VM, for the following commands this is the shell to use<br>
Write commands for update upgrade install restart enable<br>

`chmod 777 provision.sh` - turns on permissions for everything for everyone<br>
note: two ways to reference permission long form (letters) and short form(numbers), there are three sets of users, 1. user(whoever is currently logged in) 2. group of users 3. everyone else. 3 groups of 3. rwx read write execute for each group. You can use ll to see permissions<br>
`chmod +x` provision.sh changes permission on file to allow execute (sometimes you need to sudo before this command)<br>
note: you can use a https://chmod-calculator.com to find the shortform read +4 write +2 execute +1<br>

`./provision.sh to run script`<br>
note: for the following commands use sudo as a prefix<br>
`systemctl status nginx` to see if it is running<br>
`systemctl stop nginx` to stop nginx running<br>
`systemctl start nginx` to start nginx <br>
`systemctl restart nginx` to restart nginx <br>
note: we restart instead of start because when you change the config, the changes will not take effect with start, but they will take effect with restart<br>
`sudo systemctl enable nginx` to enable nginx<br>
---
## Environment variables
`printenv`<br>
note: variable is a store of data, env variable is set at an os level, it can be accessed anywhere in the system without the path being referenced<br>
`printenv USER` print the user env variable<br>
note: you can make normal variables use (variable name) = (variable data you want to store)<br>
`echo (name of variable)` print the normal variable<br>
`export` use to make variable an env variable<br>
note: this will be lost if you log out, to make it permanent you have to add it to a file<br>
`nano .bashrc` to access hidden file so we can always have our variable<br>
`export MYNAME=temi_is_persistent` add this to the end of the file<br>