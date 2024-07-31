## Objective
In this project, I will learn about and practice executing common TCP/IP
commands to display important system and network information on Windows
and Linux systems

Just as a reference to myself, adding /? in windows cmd lists all the possible
switches for a command, \--help for Linux

## Computer Information

**whoami** - the name of the user logged in, can add /groups to see all
groups the user belongs to

**hostname** - the name of the machine

**systeminfo** - fundamental information about the machine, such as OS
and BIOS versions, network cards, processor info **(Windows)**

**msinfo32** - GUI of 'systeminfo', you can also just type "system
information" in the start menu to pull this up **(Windows)\
**

**For Linux**, some system information can be found by "opening" a file
with the command line with 'cat'

For example, processor information can be found in /proc/cpuinfo, so
'cat /proc/cpuinfo' would list the file contents in the command line.
Same thing with /proc/meminfo

![image2](https://github.com/user-attachments/assets/de49178f-2754-47bb-902e-d4c4e60a97d2)


**net** - a very versatile command used, can manage users, groups,
network connections, services, etc. (i.e. net user Guest /active:no)

## IP and Network Information

**ping** - pong

**tracert** - follows the route that a packet goes through when pinging
an IP address, useful to diagnose connection issues **(Windows)**

**ipconfig** and **ipconfig /all** - to display basic and expanded
network information on **Windows**

**ip addr** or **ifconfig** for Linux

**cat /etc/network/interfaces** to find detailed information about
network interfaces

**cat /etc/resolv.conf** for DNS information

**Nslookup** - enters the nslookup tool which can be used to get
information from the DNS server, i.e. resolving domain names to IP
addresses. can also be entered seamlessly in command prompt instead of
separately entering nslookup to enter the tool

## 
