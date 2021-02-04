# Containers

### What is a Container  

* OS Level Virtualization
* Small Executable executable code , Lightweight
* Portable and Platform Independent
* Improve Utilization of Hardware more efficient than VM

![](.gitbook/assets/image%20%2833%29.png)

### Common Docker Commands

* Give Tag
* Build Image
* Images Command 
* Run a Container
* Push and Pull Commands

#### \(Alternative Rocket is used compared to Docker\)

* Docker file is the blueprint for an image  
* Image is an Immutable file that contains  everything necessary to run an application

![](.gitbook/assets/image%20%2834%29.png)

* Images are read only  ,  Writeable Layer is placed on top to write files  
* Layers can be shared between images which can save disk space and network bandwidth

### Dockerfile Instructions  

* From  : Define Base Images \(OS\)
* Run  : execute code  
* ENV :  set environment variables
* ADD and COPY : copy files and directories
* CMD : define the  default command for the execution of the container  , there can be only one command in case there are many only the last one will be executed  

### Container Registry 

*  Distribution of named container images
* PUSH  , PULL registry  

### Image Naming 

*  hostname/repository:tag
* tag can be version number  of OS version etc





