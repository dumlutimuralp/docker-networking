# Terminology

Dotcloud => Docker (Dock Worker)

To build better -> to ship better -> to deploy (run) better

Components of Docker is written in Go or Golang

Open Container Initiative : Standardize container format and runtime , vendor neutral, platform neutral (OCI operates under Linux Foundation)

Containers provide scalable, self healing and portable apps

## Installation

### Homebrew

I am using Homebrew for package management in my Macbook. 

Homebrew is a free and open-source software package management system that simplifies the installation of software on Apple’s macOS operating system. It is known as the missing package manager for macOS. 

For details on how to install it please check [here](https://brew.sh/)

### Install Docker using Homebrew

* Install Docker

Open up a MACOS terminal window and then perform the command as shown below

<pre><code>
dtimuralp-a02:/ dtimuralp$ <b>brew cask install docker</b>
</code></pre>

* Run Docker Daemon 

Navigate to Applications folder as shown below
dtimuralp-a02:~ dtimuralp$cd Applications

Then perform command below

<pre><code>
dtimuralp-a02:/ dtimuralp$ <b>open docker.app</b>
</code></pre>

You should see a Docker icon in your task bar in MACOS GUI as shown below

[](docker.png)

Docker Client ===== API Calls ===> Docker Daemon

<pre><code>
dtimuralp-a02:~ <b>dtimuralp$ docker version</b>
Client: Docker Engine - Community
 Version:           <b>18.09.2</b>
 API version:       1.39
 Go version:        go1.10.8
 Git commit:        6247962
 Built:             Sun Feb 10 04:12:39 2019
 OS/Arch:           darwin/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          18.09.2
  API version:      1.39 (minimum version 1.12)
  Go version:       go1.10.6
  Git commit:       6247962
  Built:            Sun Feb 10 04:13:06 2019
  OS/Arch:          linux/amd64
  Experimental:     true
dtimuralp-a02:~ dtimuralp$ 
</code></pre>

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

# Appendix

Using Symlinks : 
Create = > ln -s /Users/dtimuralp /awesome

Delete = > unlink /awesome
