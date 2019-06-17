# Terminology

Dotcloud => Docker (Dock Worker)

To build better -> to ship better -> to deploy (run) better

Components of Docker is written in Go or Golang

Open Container Initiative : Standardize container format and runtime , vendor neutral, platform neutral (OCI operates under Linux Foundation)

Containers provide scalable, self healing and portable apps

# Installating Docker

## Preperation

I used a single Ubuntu 16.04.5 LTS based virtual machine on VMware ESX.

* SSH to Ubuntu virtual machine and then escalate to root

<pre><code>
vmware@ubuntu26:~$ 
vmware@ubuntu26:~$ <b>sudo -H bash</b>
root@ubuntu26:/home/vmware#
</code></pre>

*  Ensure the integrity and authenticity of the images that are downloaded from Docker Hub. GPG is based on Public Key Cryptogragphy (more info is [here](https://www.gnupg.org/)

<pre><code>
root@ubuntu26:/home/vmware#<b>curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -</b>
OK
</code></pre>

* Configure Docker Hub as the APT source rather than the Ubuntu 16.04 repository

<pre><code>
root@ubuntu26:/home/vmware# <b>sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"</b>
root@ubuntu26:/home/vmware#
</code></pre>

* Update Docker packages

<pre><code>
root@ubuntu26:/home/vmware# <b>apt-get update</b>
Get:1 http://security.ubuntu.com/ubuntu xenial-security InRelease [109 kB]
Get:2 http://us.archive.ubuntu.com/ubuntu xenial InRelease [247 kB]
|
|
Output Omitted
|
|
Get:48 http://us.archive.ubuntu.com/ubuntu xenial-backports/universe Translation-en [4,184 B]
Fetched 30.0 MB in 9s (3,233 kB/s)
Reading package lists... Done
root@ubuntu26:/home/vmware#
</code></pre>

* Make sure the repository for Docker is Docker repo rather than Ubuntu repo

<pre><code>
root@ubuntu26:/home/vmware# <b>apt-cache policy docker-ce</b>
docker-ce:
  <b>Installed: (none)</b>
  Candidate: 5:18.09.6~3-0~ubuntu-xenial
  Version table:
     5:<b>18.09.6~3-0~ubuntu-xenial</b> 500
        500 https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
    |
    Output Omitted
    |
     17.03.0~ce-0~ubuntu-xenial 500
        500 https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages
root@ubuntu26:/home/vmware# ^C
</code></pre>


### Installation 

* Install Docker

<pre><code>
root@ubuntu26:/home/vmware# <b>sudo apt-get install -y docker-ce</b>
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
  aufs-tools cgroupfs-mount containerd.io docker-ce-cli libltdl7 pigz
Suggested packages:
  mountall
The following NEW packages will be installed:
  aufs-tools cgroupfs-mount containerd.io docker-ce docker-ce-cli libltdl7 pigz
0 upgraded, 7 newly installed, 0 to remove and 161 not upgraded.
Need to get 50.5 MB of archives.
After this operation, 243 MB of additional disk space will be used.
Get:1 http://us.archive.ubuntu.com/ubuntu xenial/universe amd64 pigz amd64 2.3.1-2 [61.1 kB]
Get:2 https://download.docker.com/linux/ubuntu xenial/stable amd64 containerd.io amd64 1.2.5-1 [19.9 MB]
Get:3 http://us.archive.ubuntu.com/ubuntu xenial/universe amd64 aufs-tools amd64 1:3.2+20130722-1.1ubuntu1 [92.9 kB]
Get:4 http://us.archive.ubuntu.com/ubuntu xenial/universe amd64 cgroupfs-mount all 1.2 [4,970 B]
Get:5 http://us.archive.ubuntu.com/ubuntu xenial/main amd64 libltdl7 amd64 2.4.6-0.1 [38.3 kB]
Get:6 https://download.docker.com/linux/ubuntu xenial/stable amd64 docker-ce-cli amd64 5:18.09.6~3-0~ubuntu-xenial [13.0 MB]
Get:7 https://download.docker.com/linux/ubuntu xenial/stable amd64 docker-ce amd64 5:18.09.6~3-0~ubuntu-xenial [17.4 MB]
Fetched 50.5 MB in 10s (4,785 kB/s)
Selecting previously unselected package pigz.
(Reading database ... 59693 files and directories currently installed.)
Preparing to unpack .../pigz_2.3.1-2_amd64.deb ...
|
|
OUTPUT OMITTED
|
|
Setting up aufs-tools (1:3.2+20130722-1.1ubuntu1) ...
Setting up cgroupfs-mount (1.2) ...
Setting up <b>containerd.io</b> (1.2.5-1) ...
Setting up <b>docker-ce-cli</b> (5:18.09.6~3-0~ubuntu-xenial) ...
Setting up <b>docker-ce</b> (5:18.09.6~3-0~ubuntu-xenial) ...
update-alternatives: using /usr/bin/dockerd-ce to provide /usr/bin/dockerd (dockerd) in auto mode
Setting up libltdl7:amd64 (2.4.6-0.1) ...
Processing triggers for libc-bin (2.23-0ubuntu10) ...
Processing triggers for systemd (229-4ubuntu21.4) ...
Processing triggers for ureadahead (0.100.0-19) ...
root@ubuntu26:/home/vmware#

</code></pre>

* Check the docker version : :

<pre><code>
dtimuralp-a02:Applications dtimuralp$ <b>docker version</b>
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
  Version:          <b>18.09.2</b>
  API version:      1.39 (minimum version 1.12)
  Go version:       go1.10.6
  Git commit:       6247962
  Built:            Sun Feb 10 04:13:06 2019
  OS/Arch:          linux/amd64
  Experimental:     true
dtimuralp-a02:Applications dtimuralp$
</code></pre>

Docker Client ===== API Calls ===> Docker Server (Daemon) , hence there are two docker versions are shown in this output.

* Check Docker information

<pre><code>
dtimuralp-a02:~ dtimuralp$ <b>docker info</b>
Containers: 6
 Running: 0
 Paused: 0
 Stopped: 6
Images: 6
Server Version: 18.09.2
Storage Driver: overlay2
 Backing Filesystem: extfs
 Supports d_type: true
 Native Overlay Diff: true
Logging Driver: json-file
Cgroup Driver: cgroupfs
Plugins:
 Volume: local
 <b>Network: bridge host ipvlan macvlan null overlay</b>
 Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
Swarm: <b>inactive</b>
Runtimes: <b>runc</b> 
Default Runtime: runc
Init Binary: docker-init
containerd version: 9754871865f7fe2f4e74d43e2fc7ccd237edcbce
runc version: 09c8266bf2fcf9519a651b04ae54c967b9ab86ec
init version: fec3683
Security Options:
 seccomp
  Profile: default
Kernel Version: 4.9.125-linuxkit
Operating System: <b>Docker for Mac</b>
OSType: linux
Architecture: x86_64
CPUs: 4
Total Memory: 1.952GiB
Name: linuxkit-025000000001
ID: ADFW:REBN:BHN6:OQ5Z:3OO7:CBDL:YUF7:QAQK:DKR7:RRS3:ITT4:VZVC
Docker Root Dir: /var/lib/docker
Debug Mode (client): false
Debug Mode (server): true
 File Descriptors: 24
 Goroutines: 50
 System Time: 2019-06-17T14:59:43.4842239Z
 EventsListeners: 2
HTTP Proxy: gateway.docker.internal:3128
HTTPS Proxy: gateway.docker.internal:3129
Registry: https://index.docker.io/v1/
Labels:
Experimental: true
Insecure Registries:
 127.0.0.0/8
Live Restore Enabled: false
Product License: Community Engine

dtimuralp-a02:~ dtimuralp$ 
</code></pre>

Docker Swarm shows up as inactive cause I am using a single host.

Runc is Docker' s container runtime which is also donated to the Open Container Project (OCP) . For more info please check [here](https://blog.docker.com/2015/06/runc/)

Docker Operations System is Docker for MAC since I installed it on my MAC.

* Check whether if any containers run by default

<pre><code>
dtimuralp-a02:~ dtimuralp$ <b>docker ps</b>
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
dtimuralp-a02:~ dtimuralp$ 
</code></pre>

Seems like no containers to me.

* Check whether if there is any container images in the local repository

docker images

* Check which Docker networks come by default :

<pre><code>
dtimuralp-a02:~ dtimuralp$ <b>docker network ls>/b>
NETWORK ID          NAME                DRIVER              SCOPE
b5a32321deb2        <b>bridge</b>              <b>bridge</b>              <b>local</b>
7014fee64b30        host                host                local
17f1c5830fc3        none                null                local
dtimuralp-a02:~ dtimuralp$
</code></pre>

Scope "local means, single-host mode. We will focus on the "bridge" for now.

* Inspect the "bridge" config

<pre><code>
dtimuralp-a02:~ dtimuralp$ <b>docker inspect bridge</b>
[
    {
        "Name": "bridge",
        "Id": "b5a32321deb2281ac40e2943c89acc72b2f21cd95db3062b1a9ff01b6e42d730",
        "Created": "2019-06-15T08:17:26.197777261Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "Gateway": "172.17.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]
dtimuralp-a02:~ dtimuralp$ 
</code></pre>



docker run hello-world (automatically exits the container at the end)

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
