# Containers, Docker

### What is a Container &#x20;

* OS Level Virtualization
* Small Executable executable code , Lightweight
* Portable and Platform Independent
* Improve Utilization of Hardware more efficient than VM

![](<.gitbook/assets/image (33).png>)

![](<.gitbook/assets/image (35).png>)

### Common Docker Commands

* Give Tag
* Build Image
* Images Command&#x20;
* Run a Container
* Push and Pull Commands

#### (Alternative Rocket is used compared to Docker)

* Docker file is the blueprint for an image &#x20;
* Image is an Immutable file that contains  everything necessary to run an application

![](<.gitbook/assets/image (34).png>)

* Images are read only  ,  Writeable Layer is placed on top to write files &#x20;
* Layers can be shared between images which can save disk space and network bandwidth

### Dockerfile Instructions &#x20;

* From  : Define Base Images (OS)
* Run  : execute code &#x20;
* ENV :  set environment variables
* ADD and COPY : copy files and directories
* CMD : define the  default command for the execution of the container  , there can be only one command in case there are many only the last one will be executed &#x20;

### Container Registry&#x20;

* &#x20;Distribution of named container images
* PUSH  , PULL registry &#x20;

### Image Naming&#x20;

* &#x20;hostname/repository:tag
* tag can be version number  of OS version etc

### Running Containers

```
# Check Docker is installed  
docker --version

# use docker CLI to list the images :
docker images

# pull Hello World Docker Image  
docker pull hello-world

# Run the hello World image
docker run hello-world

# List the containers to see that your container ran and exited successfully.
docker ps -a

# Remove the Docker Container
# container ID optained from above command 
docker container rm <container_id>

# Run the Following Image 
docker build . -t myimage:v1

#build from $code Dockerfile
docker build -t myfirstship .

#Run Docker Image (Static Port same)
docker run -d -p 80:80 myfirstship

#Run Another Instance of the Same thing (Random port)
docker run -d -p myfirstship

# Run the Image as Container (8888 -> 80)
docker run -p 8888:80 myimage:v1

# In another terminal ping the container
curl localhost:8080

# Stop the Docker Container
docker stop $(docker ps -q)
```

### Deleting Docker

```
- list all docker downloaded images
$ docker images -a

- Removing all the docker downloaded images
$ docker rmi $(docker images -a -q)


```

### Docker Command Sheet

```
# Install docker
sudo apt-get get install docker.io

# list Docker version
sudo docker --version

# Download and create a docker image with ubuntu latest version
sudo docker run --name myubuntu ubuntu:latest

# list all the docker images
sudo docker ps -a

# run and -it is to atach the new container terminals
sudo docker run -it  --name myubuntu1 ubuntu:latest 

# connect to docker terminal  
sudo docker attach myubuntu1 
# cntl q command to exit

# Display the working directory of the container
sudo docker exec myubuntu1 pwd

# Display the environment variable of the container
sudo docker exec myubuntu echo $PATH

# List all the Running containers
sudo docker ps -a

# List all docker ID 
docker ps -aq

# remove all docker container at once  
docker rm $(docker ps -aq)


# Delete a container   -f is for force remove
sudo docker rm -f myubuntu

# Run Container that runs in background
sudo docker run -d --name myubuntu ubuntu:latest

# Map port  
sudo docker run  -d -p 8080:80 myubunut

# docker terminal connect
docker exec -it <id> bash

# Rename docker image
docker rename <old_name> <new_name>

********************************************************

# Load project in Docker Volume
docker exec -it new_site bash
- create a folder inside for project
- Load the project
- exit the project using " exit"
 
# create a folder in local machine and create a folder
# -v is for refering to a volume -p port
docker run -d -p 8081:80 --name website2 -v $(pwd):/website nginx

# For Windows
docker run -d -p 8080:80 --name website2 -v "%cd%":/usr/share/nginx/html nginx

   
 # verify the container
 docker exec -it website2 bash
 
 --------------------------------------------------------
 
 # Create docker volume
 docker volume create my-vol
 
 #inspect docker volume
 docker inspect my-vol
 
 # Attach the docker volume to the contianer 
 # mapping multiple ports
 # adding the volume <vol_name>:path in container to copy volume data 
 docker run -d --name myimage -p 5000:5000 -p 8087:8000 -v my-vol:/var/jenkins myImage
 
 
 ---------------------------------------------------------------------
 
 # Copy Data from container to Host
 
 # Create the folder
 mkdir data
 
 # copying the data from container to host
 docker cp myImage:/var/jenkins_home ./data
 
 --------------------------------------------------------------------
 
 # Create Image tar file from docker image in current path
  docker save myImage/myapp > webapp.tar
  

 
 # Load Tar file as docker image
  docker load -i webapp.tar
  
 ---------------------------------------------------------------------
  
 # Create Image Tar of the system
 
 # create an ubuntu image
 docker run -it --name myUbuntu ubuntu
 
 # Create Tar file
 docker export myUbuntu > newsystem.tar
 
 # Unzip from Tar file back
 sudo docker import - myImnage/ubuntu < newsystem.tar 
```

## &#x20;Troubleshooting and Monitoring Containers

```
# Stats for the docker
docker stats
docker logs <id>
```

```
# Docker Run Python 3.6
docker --rm -ti python:3.6 python

# Docker Run Python 2.7
docker run --rm -ti python:2.7 python

# Run Jupyter Notebook
docker run --rm -p 8888:8888 jupyter/scipy-notebook
```

## Docker CheatSheet

```
// Destory all 
$ docker prune

// run hello world
$ docker run hello-world

```

## Container Orchestration

* manage lifecycle of containers ,in dynamic environments
* using Kubernetes&#x20;

### Kubernetes Architecture

![](<.gitbook/assets/image (36).png>)

### Labels / Selectors and Namespace in Cluster

![](<.gitbook/assets/image (37).png>)

### Pod in Cluster

* Wrapper for a Single Container
* Replica set for Horizontal Scaling
* specified using YAML file

Imperative Commands

Declarative Commands

* &#x20;Apply Command&#x20;



* Use the `kubectl` CLI
* Create a Kubernetes Pod
* Create a Kubernetes Deployment
* Create a ReplicaSet that maintains a set number of replicas
* Witness Kubernetes load balancing in action

```
# Verify the kubectl
kubectl version

#kubectl requires configuration so that it targets the appropriate cluster. Get cluster information with the following command:
kubectl config get-clusters

# A kubectl context is a group of access parameters, including a cluster, a user, and a namespace. View your current context with the following command:
kubectl config get-contexts

#List all the Pods in your namespace
kubectl get pods

#describe pods
kubectl describe pod hello-world

# Delete the Pod
kubectl delete pod hello-world

# Imperatively create a Pod using the provided configuration file.
kubectl create -f hello-world-create.yaml

#Use the kubectl apply command to set this configuration as the desired state in Kubernetes
kubectl apply -f hello-world-apply.yaml

#List Services in order to see that this service was created
kubectl get services

#Delete the Deployment and Service
kubectl delete deployment/hello-world service/hello-world
```

#### Managing Applications with Kubernetes

```
# Build an Image  
docker build -t us.icr.io/$MY_NAMESPACE/hello-world:1 . && docker push us.icr.io/$MY_NAMESPACE/hello-world:1

# Run Image as Deployment
kubectl apply -f deployment.yaml

# Access the Application  
kubectl expose deployment/hello-world --type=NodePort --port=8080 --name=hello-world --target-port=8080

# Scale  up deployment
kubectl scale deployment hello-world --replicas=3

#scale down deployment
kubectl scale deployment hello-world --replicas=1

# Getting Rollout Update
kubectl rollout status deployment/hello-world

# Create a ConfigMap that contains a new message
kubectl create configmap app-config --from-literal=MESSAGE="This message came from a ConfigMap!"

# Build and push a new image that contains your new application code
docker build -t us.icr.io/$MY_NAMESPACE/hello-world:3 . && docker push us.icr.io/$MY_NAMESPACE/hello-world:3

# delete the old ConfigMap and create a new one with the same name but a different message
kubectl delete configmap app-config && kubectl create configmap app-config --from-literal=MESSAGE="This message is different, and you didn't have to rebuild the image!"

# Restart the Deployment so that the containers restart
kubectl rollout restart deployment hello-world

# Delete the Deployment
kubectl delete -f deployment-configmap-env-var.yaml
```

### Openshift Architecture

![](<.gitbook/assets/image (38).png>)

### Source to Image

![](<.gitbook/assets/image (39).png>)

### Build Triggers

![](<.gitbook/assets/image (40).png>)

### Istio

![](<.gitbook/assets/image (41).png>)

### Cloud Native computing Foundation

![](<.gitbook/assets/image (42).png>)

### Redhat Openshift (Code snippets)

* Use the `oc` CLI
* Use the OpenShift web console
* Build and deploy an application using s2i
* Inspect a BuildConfig and an ImageStream

```
# Check OC version
oc version

# List the pods in the Namespace
oc get pods

# In addition to Kubernetes objects, you can get OpenShift specific objects.
oc get buildconfigs

# View the OpenShift project that is currently in use.



```



### Reference&#x20;

* [ How does 'kubectl exec' work?](https://erkanerol.github.io/post/how-kubectl-exec-works/#.YMh9R3cvFUs.twitter)



