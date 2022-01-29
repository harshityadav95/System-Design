# Linux Directories

&#x20;

![](<.gitbook/assets/image (43).png>)

## Navigate&#x20;

```
cd 
ls
```

## /Bin

* &#x20;Binaries and Executables

## /SBin

* &#x20;essential executables for super user (root)

## /Lib

* shared between binaries
* shared between sbin and bin binaries

## /usr/bin

* non essential user installed libraries

## /usr/local/bin

* locally compiled binaries &#x20;
* safe place to install packages that wont conflict with the user installed  location

## $Path

* &#x20;tell linux where to find executables

## which \<binary\_name>

* eg which curl , which cd&#x20;
* tells its full path in the file system

## /ETC

* Editable text config
* config files to modify settings for installed software's and preferences

## /Home/\<user\_data>

* Each user registered in the system will have folder in the home directory &#x20;

## \~&#x20;

* &#x20;File path to root
* starting point&#x20;

## /Boot

* Boot system &#x20;
* contains the linux kernel

## /Dev

* device files
* Interface with hardware of drivers as if they were device files
* Create Disk partition here &#x20;
* Interact with Floppy drive

## /Opt

* &#x20;optional directory &#x20;
* for optional software  (rare interaction)

## /var

* Variables files
* changes as you use the OS &#x20;
* contains the logs and the cache file&#x20;

## /Tmp&#x20;

* temp files that wont be persisted between the reboots
* non persistant storage

## /Proc

* Illusionary File System that doesnt exist on disk
* created in memory by linux kernal to keep track of running process









