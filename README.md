# Terminology

Dotcloud => Docker (Dock Worker)

To build better -> to ship better -> to deploy (run) better

Components of Docker is written in Go or Golang

Open Container Initiative : Standardize container format and runtime , vendor neutral, platform neutral (OCI operates under Linux Foundation)

Containers provide scalable, self healing and portable apps

Overall service is called registry, then in the registry we can have our own repositories (repo in short)

Docker machine : Provisions docker hosts/engines
Docker compose : Compose multi container apps
Docker swarm : Scheduled containers over multiple docker engines
Tutum : GUI to deploy infra to on prem or public cloud


## A standard installation

DOCKER CLIENT => talks to (API CALLS)  => Docker DAEMON

docker version

docker info

docker run hello-world (automatically exits the container at the end)

docker ps

docker ps -a (shows the stopped containers as well)

docker images (or docker images ls)

docker pull ubuntu

docker rm <container-id>

docker rmi <imagename> (or docker rm <image-id>)

docker run -d --name testcontainer -p 80:8080 ubuntu
(-d means detached mode, no need to latch it to the terminal of docker host itself)
(80 is host s port, 8080 is container s port)

docker run -it --name testcontainer ubuntu:latest /bin/bash (To properly exit, without killing the container, ctrl + P + Q)

docker stop $(docker ps -aq)

docker rm $(docker ps -aq)

docker rmi $(docker images -q)

docker login

docker tag bec7434da28d dumlutimuralp/nsxdemo:v1
Note : bec7434da28d is the image id from docker images output

docker push dumlutimuralp/nsxdemo:v1

* Edit an existing container by connecting to it via bash shell and then use below command  :
docker commit container-ID image-name

## ANALOGIES
images can be interpreted as "stopped containers" (like VM Templates)
containers can be interpreted as "running images" (like a VM template turned into an actual VM)

##  RECAP

*docker run : starts a new container (from a referenced image)
*docker pull : copies images to docker host
*docker images : lists images on the docker host
*docker rmi : removes images from the docker host 
*docker ps : lists the running containers
*docker stop : stops running container
*docker rm : removes stopped container


# Networking Basics

docker network 

docker network ls

docker network inspect <name> (or <network-id>)

docker info (shows network plugins)


## Bridge

docker network create -d bridge --subnet 10.0.0.1/24 testbridge

"bridge-utils" package helps us to examine linux bridge features. (need to be installed seperately)

ip link show

docker run -dt --name dumlu1 --network testbridge alpine sleep 1d

docker exec -it dumlu1 sh

docker run -d --name dumlu --network testbridge -p 5000:8080 nigelpoulton/pluralsight-docker-ci

docker port dumlu
