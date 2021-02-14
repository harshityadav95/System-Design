# Linux Directories

 

![](.gitbook/assets/image%20%2845%29.png)

## Navigate 

```text
cd 
ls
```

## /Bin

*  Binaries and Executables

## /SBin

*  essential executables for super user \(root\)

## /Lib

* shared between binaries
* shared between sbin and bin binaries

## /usr/bin

* non essential user installed libraries

## /usr/local/bin

* locally compiled binaries  
* safe place to install packages that wont conflict with the user installed  location

## $Path

*  tell linux where to find executables

## which &lt;binary\_name&gt;

* eg which curl , which cd 
* tells its full path in the file system

## /ETC

* Editable text config
* config files to modify settings for installed software's and preferences

## /Home/&lt;user\_data&gt;

* Each user registered in the system will have folder in the home directory  

## ~ 

*  File path to root
* starting point 

## /Boot

* Boot system  
* contains the linux kernel

## /Dev

* device files
* Interface with hardware of drivers as if they were device files
* Create Disk partition here  
* Interact with Floppy drive

## /Opt

*  optional directory  
* for optional software  \(rare interaction\)

## /var

* Variables files
* changes as you use the OS  
* contains the logs and the cache file 

## /Tmp 

* temp files that wont be persisted between the reboots
* non persistant storage

## /Proc

* Illusionary File System that doesnt exist on disk
* created in memory by linux kernal to keep track of running process











