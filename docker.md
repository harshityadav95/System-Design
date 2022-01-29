# Docker

#### Check the Docker version&#x20;

```
docker version
```

### Run Hello World

```
docker run hello-world
```

### Docker Details

```
docker ps -a
docker ps
docker images
docker info

```

### Run Ubuntu&#x20;

```
docker pull ubuntu

docker run -it ubuntu 

$apt-get update

exit 

docker container ls -all
 
```

### Commit the Changes

```
docker commit -m "did a apt-get udpate" -a "authorName" funny_meninsky harshit/ubuntu-demo
```

## Docker Swarms

```
docker swarm init --advertise-addr 192.168.29.44:2377 --listen-addr 192.168.29.44:2377
```

Did not work port errir

### Install Minicube on Mac

```
brew install kubectl
brew install minikube
```

### Kuberenets Commands&#x20;

```
# command structure

kubectl [command] [type] [name] [flags]

## Make local + external nodes visible
$ kubectl get nodes


## version of both local and client version
$ kubectl version
```

Q . Version difference between client and server version .

* Get the Deployments (Only deployed)

```
$ kubectl get deployments
```

* Get pods both the deployed and un-deployed ones

```
$ kubectl get pods
```

* &#x20;Get the Details of the Pod in YAML format

```
$ kubectl get pod <pod name> -o=yaml    
```

* All the Details of pods and deployments etc .

```
$ kubectl get all
```

* Get all the namespace in cluster

```
$ kubectl get namespaces
```

* Get all the namespaces with Labels in the cluster

```
$ kubectl get namespaces --show-labels
```

* Create Namespace in cluster

```
```

Q . difference between context and namespace in context



```
$ kubectl 
```
