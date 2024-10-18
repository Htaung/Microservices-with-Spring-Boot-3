Todo:aa 
https://www.freecodecamp.org/news/bash-scripting-tutorial-linux-shell-script-and-command-line-for-beginners/#heading-introduction

D:\From External Drive\Udemy Courses\[FreeCourseLab.com] Udemy - Linux Administration Bootcamp Go from Beginner to Advanced\3. Linux Fundamentals\7. File and Directory Permissions Explained - Part One


https://www.freecodecamp.org/news/bash-scripting-tutorial-linux-shell-script-and-command-line-for-beginners/#heading-introduction
How to Schedule Scripts using cron => 9 March 2024 stop


Page 76  need to learn about bash script
D:\LearningProjects\2-basic-rest-services\test-em-all.bash


Page 80 To page 102 will skip of  Deploying Our Microservices Using Docker as I don't have own laptop

Gradle build jar located in D:\LearningProjects\2-basic-rest-services\microservices\product-service\build\libs

groups
groups userName
id -gn

ugoa => user, group, other, All
chmod u+rw
chmod u+rw,g+r
chmod a=r
chmod u=rwx,g=r,o= 

r w x
4 2 1 => base 10 values for on

				U 	   G 		O
symbolic =>    rwx	   r-x      r--	
binary 	 =>    111     101		100	
Decimal	 =>		7		5		4


-rwxr-xr-x => 755
-rw-rw---- => 660

chgrp change the group

directory permission can affect on file

umask -S file creation mode or folder creation mode => File Creation Mask
0022

7 7 7
0 2 2
7 5 5 => 0022

umask 007 


find -name pattern
-iname pattern => like name ignore case 
-ls => performs ls on each of found items

-mtime days => days old
-size num => 
-newer file => newer than File

-exec command {} \;
run commands against all the files that are found

# Finding File and Directory
 find /usr/sbin/ -iname Run	
 
 find /usr/sbin/ -name *init
 
 10 days old and less than 13 days
 find . -mtime +10 -mtime -13 -ls
 find . -name *s -ls
 find . -size +1M
 
 # Find any in Directory that have aa.txt
  find .  -type d -newer aa.txt

find . -exec file {} \:
find . -exec file {} \:


locate 

find abosolute_path -name *.txt

#find abosolute_path -name *.txt execute command file
 find /home/aung/bash_test/ -name  *.txt -exec file {} \;
 # command file 
 # placeholder {}
 # delimeter \; and +
 
 find /home/aung -name "*.txt" -exec echo {} \;


9. Finding Files and Directories



vi file.txt
Esc + yy => copy current line (yank)
Esc + p => paste
Esc+10i<text>Esc => repeat text 10 times
Esc+:Lineno => goto line no
Esc+$ => go to end of line
Esc+cc => change entire line

Removing File and Directory
rm file => Remove file
rm -r dir => remove dir and it contents recursively
rm -f file => force removal and never prompt for confirmation

cp -r dir dir2 => if dir2 not exist create automatically
cp -r dir dir2 dir3 => copy dir and dir2 to dir3 recursively

mv dir aundir => rename
sort file => sort the file content
sort -u file => unique
sort -ru file => reverse and unique

Creating a collection of file
tar 
c => create a tar archive
x => extract files from archive
t => list
v => verbose logs
z => use compression
f file => use this file

tar cf tps.tar directory
tar tf tps.tar
tar xf tps.tar 
tar czvf tps.tar.gz dir

disk usage
du
du -k  => kilobype
du -h  => human readble





Wild cart
[aeiou]*
[a-g]
[3-5]
[[:alpha:]]
[[:alnum:]]
[[:digit:]]
[[:lower:]]
[[:space:]]
[[:upper:]]




IO
stdin  0
stdout 1
stderr 2

Redirection
> override existing contents Redirect stdout to file
>> appends to any existing contents
< Redirect input from file to command

& used with redirection to signal that file descriptor is being used
2>&1 combined stderr and stdout 
2>file redirect stderr to file

The Null Device
>/dev/null redirect Output to nowhere
ls -l > file.txt

sort < file.txt
sort < file.txt > sort_file.txt

ls file.txt no-here > out

ls file.txt not-here 2> out.err
ls file.txt not-here 1> out 2> out.err

ls file.txt not-here > out.both 2>&1

ls file.txt not-here > /dev/null

#hide error msg stderr to /dev/null
ls file.txt not-here 2> /dev/null

#Ctrl - w w go to next windows
vimdiff file1 file2
diff file1 file2
sdiff file1 file2

grep -i #ignore case
grep -c #count the no. of occurences in File
grep -n  #precede output with line no.
grep -v #invert match. print line don't match
grep -ci aung file.txt

file file_name display file type

#Pipe
| #comand-out | command-input

cat file | grep pattern

cut file => cut out selected portion of file.if file is ommited user std in
cut -d => delimeter
cut -f N => display Nth field

strings file_name # convert to string

strings aa.mp3 | grep -i jhon | head -1

#-f2 second fields
strings aa.mp3 | grep -i jhon | head -1 | cut -d ' ' -f2

grep aung /etc/passwd

#field 1 to column 5
grep aung /etc/passwd | cut -d: -f1,5 | tr ":" " "

# colume -d table format tr => replace ":" with " "
 cat /etc/passwd | sort | cut -d: -f1,5 | tr ":" " " | column -d
 
 #piping output to pager
 more
 less 
 
 #txfer file over network
 SCP
 SFTP
 
 #scp client
 scp
 sftd
 PUTTY secure copy pscp.exe
 PUTTY sftp => psftp.exe
 
 scp source targer
 sftp host
 sftp username@host
 ftp host => txfer session with host
 
 sftp host
 put file.txt upload to server
 
 #Using alias
 short cut
 alias name=value
 alias cls=clear
 
 to make permanent alias
 add in .bash_profile
 add in .bash_rc
 add in .profile
 
 #ENV var
 name=value 
 printenv
 
 #creating env 
 export HOST="aung"
 
 #removing env
 unset HOST
 
 Persisting ENV
 add in .bash_profile
 add in .bash_rc
 add in .profile
 
 export TZ="" #timezone
 

 
 
 ps
 -e everything, all process
 -f full format listing
 -u username's process 
 -p processid
 
 ps -eH display a process tree
 ps -e --forest
 ps -u userName
 
 pstree display process tree
 top interactive process viewer
 htop interactive process viewer
 
 ps -p processid
 ps -fu root
 
 bg %jobno background a suspended process
 fg %jobno foregroud a background process
 jobs list job

kill -l list of signals to kill process
jobs 
jobs %no.
jobs %% current jobs
jobs %+ current jobs
jobs %- previous job 
 kill %jobno
 kill processid
 kill -9 processid
 
 #Scheduling repeated jobs with cron
 cron  time based job Scheduling
 crontab program to CRUD job schedule
 
 crontab format
 * 			* 				* 						* 						*
 mm(0-59) hh(0-23) days_of_the_month(1-31) month_of_the_year(1-12) 	day_of_the_week(0-6)
 
 #Every mon day at 7am
 0 7 * * 1
 
 0/30 every 30 mins
 */2 every 30 mins 
 0-4 first 5 mins of the hour => 5 times 
 
 0 0 1 1 * yearly
 0 0 1 1 * annualy
 0 0 1 * * monthly
 0 0 * * 0 weekly
 0 0 * * * daily
 0 0 * * * mid night
 0 * * * * hourly
 
 crontab file  => install new crontab from file
 crontab -l    => list
 crontab -e    => edit
 crontab -r    => remove 
 
 vi aa-cron
 # Run every Monday at 07:00
0 7 * * 1  /home/aung/bash_test/for_loop.sh

crontab aa-cron => create
crontab -e => edit 
crontab -r => delete


# 12. Switching Users and Running Commands as Others
Switch to another account
run command as other

#change userid or become super user
su - userName

export TEST=1

#How to Add a New User and Create Home Directory  => sudo useradd -m jane => -m (--create-home)
#Creating a User with a Specific Home Directory => sudo useradd -m -d /home/oracle oracle
sudo useradd oracle
sudo passwd oracle 
sudo chown oracle /home/oracle

su -c 'echo $ORACLE_HOME' - oracle
sudo -l #list of available command
sudo -u root
sudo -u userName
sudo su -
sudo su - userName
sudo -s #start shell
sudo -u root -s #start sudo -s
sudo -u user -s #start shell as user

sudo -u oracle /opt/tomcat/bin/start.sh # Run as oracle user 

#13. Shell History and Tab Completion
shell History
~/.bash_history
~/.history
~/.histfile

#history size
echo $HISTSIZE

! => called it as bang 
!N => repeat command line number N
!! => Repeat previous command line
!string => Repeat most recent command start with string

vi !:2 =>:2 argument

head files.txt sort_file.txt note.txt
!^ => !:1 => file.txt 
!$ => last argument => note.txt

Ctrl-r => Reverse shell directory
Ctrl=g => cancel 

cd ~username => go to home directory

#14. Installing Software
package => collection of file, data, metadata, version, dependency
package manager => install,upgrade,remove, manage dependency,keep track

RPM Distro=> Red hat package manager => yum, rpm
Redhat, CentOs,Fedora,Oracle linux, Scientific Linux
=> yum
=> rpm -qa #List
=> which java
=> rpm -qf /opt/tomcat/bin/ list file's package
=> rpm -ql package => list package file
=> rpm -ivh package.rpm  => install package
=> rpm -e => uninstall package

yum search string 
yum info [package]
yum install [-y] package
yum remove [package] 

Deb Based Distro=> Debian, Linux Mint, Ubuntu => apt-get, dpkg
APT => advance Packaging tool
apt-cache search string #Search String 
apt-get install -y package #install package
apt-get remove package => Remove package, leaving config
apt-get purge package => Remove package, deleting config


dpkg -l => list installed package
dpkg -S /path => List file package

dpkg -L package => list all file in package
dpkg -i package.deb  => install package



#Linux Boot process
BIOS
Boot Loader
Linux Kernel
Run Level

BIOS
Special firmware
OS Independent
Primary purpose is to find and execute boot loader
Performs POST => Power on Self TEST
knows about bootable device => hard disk,usb drive, ...
boot device search order can be change

Boot Loaders
LILO => linux loader
GRUB => Grand Unified BootLoader, Replace LILO
-start the OS
-start the OS with different options

Initial RAM disk
-initrd => Initial ram disk
- temporary file system that is loaded from disk and stored in memory
- contains helper and modules requried to load permanent OS file system

/boot directory
- contains the file requried to boot linux
- initrd
- kernel
- boot loader config

Kernel Ring buffer
contains msg from linux kernel, dmesg, /var/log/dmesg


Run level description
0 => Shutdown 
1,S,s => Single User mode, used for maintenance
2 => Multi-user mode with gui (Debian,Ubuntu)
3 => Multi-user text mode
4 => undefined
5 => Multi-user mode with gui (Redhat,CentOS)
6 => Reboot

Init
/etc/inittab: 
id:3:initdefault:
being phased out by systemd

Systemd 
Use target instead of run level
#/lib/systemd/system
#ls -l runlevel5.target
runlevel5.target -> graphical.target
systemctl set-default graphical.targert

Changing runlevels or target
#telinit RUNLEVEL
telinit 5
#systemctl isoloate target
systemctl isolate graphical.target

Reboot
telinit 6
systemctl isolate reboot.target
reboot

Rebooting
shutdown option time msg
shutdown -r 15:30 "rebooting!"
shutdown -r +5 "rebooting!"
shutdown -r now

power off
telinit 0
systemctl isolate poweroff.target
poweroff



#contains msg from linux kernel
dmesg -T | less => human readble 



systemctl get-default
systemctl set-default graphical.target


systemctl isolate graphical.target

#System Logging

syslog standard
- aid in processing of msg
- allows logging to be centrally controlled
- use facilities and severities to categorize msgs

0 => kern => kernal msg
1 => user
2 => mail
3 => daemon => system daemon
4 => auth => security auth msg
5 => syslog => msg generated by syslogd
6 => lpr => line printer subsystem
7 => news => network news subsystem
8 => uucp => uucp subsystem
9 => clock => daemon
10 => authpriv => security/authorization msgs
11 => ftp 
...
15 => cron
...
23

#syslog servers
process syslog msg based on rules
syslogd
rsyslog
syslog-ng

#rsyslog
/etc/rsyslog.conf
/etc/rsyslog.d

$IncludeConfig /etc/rsyslog.d/*.conf

#Logging rules
Selector Fields


FACILITY.SEVERITY

mail.* => same config
mail   => same config
FACILITY.none

#multple facility selection
FACILITY_1.SEVERITY; FACILITY_2.SEVERITY

#ACTION FIELD
Determine how a msg is processed

#caching vs non-caching
caching is used if the path starts with a hypen
mail.info -/var/log/mail.info

lose some msg during system crash if used in caching mode
using caching can improve i/o performance

logger [option] msg

#[option]
-p FACILITY.SEVERITY
-t TAG

#Log into mail.info with tagging mailtest msg "Test."
logger -p mail.info -t mailtest "Test."

#Retrive 1st line of mail.log
sudo tail -1 /var/log/mail.log

#logrotate
/etc/logrotate.conf

include /etc/logrotate.d

Exmaple logrotate conf
weekly
rotate 4
crate 
compressed

test logrotate config
#f force v verbose
logrotate -fv /etc/logrotate.conf

#to see default conf 
cat /etc/logrotate.d/rsyslog

#Disk Management
Partitions
MBR
GPT
Mount Points
fdisk


Disk can be divided into parts called Partitions.
Partitions allow you to separate data
1)OS,2)Application,3)User,4)Swap
1)OS,2)User home directory
As system admin you decide

#to check partition data 
df -h 

MBR 
=> can only address 2TB of disk space
=> can be up to 4 Primary Partitions
=> being phased out by GPT => GUID partition table
=> extended partition allow you to create logical partition


GPT 
=> GUID partition table
GUID => Global Unique Identifier
Replacing MBR partition scheme
Part of UEFI => Unified Exetensible Firmware Interface
UEFI is replacing BIOS
support up to 128 partition
up to 9.4 ZB disk sizes
not supported by older os 
require newer or speical tools

Mount Points
A dir used to access data on partition
/ is mount point
/home
/home/jason is on partition mounted on /home

Mounting over existing data
mkdir /home/aung
mount /dev/sdb2 /home
#you will not be able to see /home/aung

unmount /home
#you can see /home/aung again

#fdisk
alternative => gdisk , parted
fdisk not support GPT
fdisk /path/to/device



#list of devices
fdisk -l | less

fdisk /dev/sda


File System
ext= extended file system
ext2,ext3, ext4 are later release

Other File System
ReiserFS
JFS
XFS
ZFS
Btrfs

to create file System
mkfs -t TYPE Device

mkfs -t ext3 /dev/sda1
mkfs -t ext4 /dev/sda1

ls -l /sbin/mkfs*

MOUNTING with Mount
mount DEVICE MONUT_POINT
mount /dev/sdb2 /opt

in order to make mount persistent
add entry in /etc/fstab

umount /opt
umount /dev/sdb1

Preparing swap space
mkswap /dev/sdb1

#enable swap partition
swapon /dev/sdb1

#list swap device
swapon -s

#viewing label and uuid
lsblk -f 
blkid



LVM
Logical volume manager


easily resize storage while online
online data relocation
easily migrate data from one storage device to another while online

snapshot
Create point in time snapshot of your file systems - consistent backup

Skip
D:\From External Drive\Udemy Courses\[FreeCourseLab.com] Udemy - Linux Administration Bootcamp Go from Beginner to Advanced\7. LVM - The Logical Volume Manager

#User Management
format of /etc/passwd
userName:password:UID:GID:comments:home_dir:shell

root:x:0:0:root:/root:/bin/bash

custom for username => < 8 characters
ps -fu aung 

#enctyped passwd are store in etc/shadow
sudo su
cat /etc/shadow

uid  stored in /etc/login.defs

user can switch group by using newgrp
newgrp for new group

list of available shells are /etc/shells
99999 user dont't need to change passwd
aung:$y$j9T$ZW4XzvqDC7bYqGG94CPZy1$MfvLAbbGRTIy2KG.9Ezn22q5ta1V2O0vO.FCeJo0.KA:20003:0:99999:7:::

to prevent interactive use of an account, use /usr/bin/nologin or /bin/false as shell



useradd option userName
-c "Comments"
-m Create home dir
-s /shell/path => the path to user's shell



passwd aung => change pwd or create new pwd
tail -1 /etc/shadow  #store encryped password
tail -1 /etc/passwd
useradd aung -g grpName

useradd -c "Grant" -m -s /bin/bash grant
passwd grant

useradd -c "Kaung" -m -s /bin/bash -g aung -G aaProject
useradd -c "Kaung" -m -s /bin/bash -g aung

/etc/skell contains shell config(.profile,.bashrc,etc)

More userAdd options
-r Create system account
-d /home/dir => specify home Directory
userdel userName

#delete home dir 
userdel -r grant

#to change existing user account
usermod options userName
usermod -c "Aung Aung" aung

-c "COMMENTS" comments account
-g GROUP specify default group
-G GROUP1,GRP1 Additional GRP1
-s /shell/path Path to user's shell

grep mysql /etc/passwd
mkdir /opt/apache1
useradd -c "Apache web server user" -d /opt/apache1 -r -s /usr/sbin/nologin apache1


useradd -c "Apache web server user" -d /opt/apache1 -r -s /usr/sbin/nologin apache1
useradd -c "Apache web server user" -d /opt/apache1 -r -s /bin/bash apache2


#Format of /etc/group file:
grp_name:pwd:GID:account1,accountN

adm:x:4:syslog,aung
root:x:0:
aung:x:1000:

#to get group by username
groups root

#to create group 
groupadd web
tail -1 /etc/group

#to delete
groupdel grp_name

#to change existing group
groupmod [options] gp_name
-g GID
-n rename group 

groupadd writers
groupadd tv
groupadd movie
tail -3 /etc/group

useradd -c "Carlton Cuse" -g writers -G tv -m -s /bin/bash ccuse
passwd ccuse
groups ccuse

useradd -c "David fury" -g writers -G tv -m -s /bin/bash fury
passwd fury
groups fury

useradd -c "Matt Damon" -g writers -G movies -m -s /bin/bash damon
passwd damon
groups damon

useradd -c "Ben Affleck" -g writers -G movies,tv -m -s /bin/bash affleck


#network

#TCP/IP
TCP => controls data exchange
IP => sends data from one device to another
HOSTS => devices on a network that have an IP Address

#IP Networking
IP Address => exmaple => 192.168.1.1
subnet mask => exmaple => 255.255.255.0
brocast address => exmaple => 192.168.1.1
octlet.octlet.octlet.octlet => octlet value can be from 0 t0 255 => 8bits

first part => network Address
second => host address
Address Classes => used to determine the network address and host address

#Classful Network
Class A => 1.0 => 127.0 => ex. 17.24.88.9 => allowed hosts => 16,777,216
Class A => 128.0 => 191.255 => ex. 183.194.46.31 => allowed hosts => 65536
Class C => 192.0.0 => 233.255.255 => ex. 199.83.131.86 => allowed hosts => 255

#Subnet masks
Class A => 255.0.0.0 => first one is network addess and last 3 is host address
Class B => 255.255.0.0 => first and second is network address and last 2 is host address
Class C => 255.255.255.0 => first 3 is network address and last 1 is host address

#Broadcast Address, Classless Inter-Domain Routing / CIDR
check in below link 
D:\From External Drive\Udemy Courses\[FreeCourseLab.com] Udemy - Linux Administration Bootcamp Go from Beginner to Advanced\9. Networking\1. TCPIP Networking for Linux System Administrators.mp4

#Reserved Private Address space => Non Routable private network
		   Range					 => Private Address Space	
Class A => 1.0.0.0 - 127.255.255.255 => 10.0.0.0 - 10.255.255.255
Class A => 128.0.0.0 - 191.255.255.255 => 172.16.0.0 - 172.31.255.255
Class C => 192.0.0.0 - 233.255.255.255 => 192.168.0.0.0 - 192.168.255.255

#DNS and host address

#Determine ip address
ip address
ip addr
ip a 
ip address show 
ip a s
ifconfig


lo   => loop back address => 127.0.0.1/8
eth0 => actual h/w address


#DNS hostnames
FQDN => fully qualified domain name => webprod01.mycompany.com
TLD => .com,.net,.org, etc
DOMAINS => left of TLD => google
sub domains => left of domain => webprod01.ny.us.mycompany.com

#get host name
hostname
uname -n

#fully qualified name
hostname -f 

#setting hostname
hostname webprod1
echo 'webprod1' > /etc/hostname
vi /etc/sysconfig/network

#Resolving dns name
host www.google.comho

/etc/hosts is local to your linux system

#NSS Name Service Switch => Controls the search order for resolutions
/etc/nsswitch.conf

hosts: files dns
hosts: file nis dns

Network ports
1 => 1023 are well know ports
22 => SSH
25 => SMTP
80 => HTTP
143 => IMAP
389 => LDAP
443 => HTTPS


/etc/services
=> Map port names to port numbers
ssh 22/tcp

DHCP => dynamic host configuration protocol
DHCP Servers assigns ip address to DHCP clients
ip address, netmask, gateway, DNS Servers
Each ip is leased from the pool of IP Address the DHCP server manages

configuring dhcp client - RHEL
/etc/sysconfig/network-scripts/*

configuring dhcp client on Ubuntu
/etc/network/interfaces

#Assigning static ip - RHEL
/etc/sysconfig/network-scripts/ifcfg-eth0

#manualy assign ip address
ifconfig NETWORK_DEVICE addr netmask subnet-mask
ifconfig eth0 10.11.12.13
ifconfig eth0 10.11.12.13 netmask 255.255.255.0
ifconfig eth0 up

ifup and ifdown can be used instead



ping host
ping -c 3 host #3 package to host

rtt => round trip time 

traceroute -n google.com
#translate ip to dns name, if you see * => router is blocking

tracepath -n google.com

#Packet sniffing with tcpdump
tcpdump

-n Display numerical address and port
-A ASCII text output
-v
-vvv

telnet host portno



D:\From External Drive\Udemy Courses\[FreeCourseLab.com] Udemy - Linux Administration Bootcamp Go from Beginner to Advanced\10. Advanced Linux Permissions
