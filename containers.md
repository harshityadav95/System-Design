# Containers

### What is a Container  

* OS Level Virtualization
* Small Executable executable code , Lightweight
* Portable and Platform Independent
* Improve Utilization of Hardware more efficient than VM

![](.gitbook/assets/image%20%2834%29.png)

![](.gitbook/assets/image%20%2835%29.png)

### Common Docker Commands

* Give Tag
* Build Image
* Images Command 
* Run a Container
* Push and Pull Commands

#### \(Alternative Rocket is used compared to Docker\)

* Docker file is the blueprint for an image  
* Image is an Immutable file that contains  everything necessary to run an application

![](.gitbook/assets/image%20%2837%29.png)

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

### Running Containers

```text
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

# Run the Image as Container
docker run -p 8080:8080 myimage:v1

# In another terminal ping the container
curl localhost:8080

# Stop the Docker Container
docker stop $(docker ps -q)
```

### Docker Command Sheet

```text
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

# Delete a container  
sudo docker rm -f myubuntu

# Run Container that runs in background
sudo docker run -d --name myubuntu ubuntu:latest



```

## Container Orchestration

* manage lifecycle of containers ,in dynamic environments
* using Kubernetes 

### Kubernetes Architecture

![](.gitbook/assets/image%20%2833%29.png)

### Labels / Selectors and Namespace in Cluster

![](.gitbook/assets/image%20%2836%29.png)

### Pod in Cluster

* Wrapper for a Single Container
* Replica set for Horizontal Scaling
* specified using YAML file

Imperative Commands

Declarative Commands

*  Apply Command 



* Use the `kubectl` CLI
* Create a Kubernetes Pod
* Create a Kubernetes Deployment
* Create a ReplicaSet that maintains a set number of replicas
* Witness Kubernetes load balancing in action

```text
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

```text
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

![](.gitbook/assets/image%20%2838%29.png)

### Source to Image

![](.gitbook/assets/image%20%2839%29.png)

### Build Triggers

![](.gitbook/assets/image%20%2841%29.png)

### Istio

![](.gitbook/assets/image%20%2840%29.png)

### Cloud Native computing Foundation

![](.gitbook/assets/image%20%2843%29.png)

### Redhat Openshift \(Code snippets\)

* Use the `oc` CLI
* Use the OpenShift web console
* Build and deploy an application using s2i
* Inspect a BuildConfig and an ImageStream

```text
# Check OC version
oc version

# List the pods in the Namespace
oc get pods

# In addition to Kubernetes objects, you can get OpenShift specific objects.
oc get buildconfigs

# View the OpenShift project that is currently in use.



```









