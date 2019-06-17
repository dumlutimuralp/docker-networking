This guide provides the basic steps on installing Docker Community Edition on Ubuntu 16.04 and also basic networking commands in Docker.

# Terminology

Dotcloud => Docker (Dock Worker)

To build better -> to ship better -> to deploy (run) better

Components of Docker is written in Go or Golang

Open Container Initiative : Standardize container format and runtime , vendor neutral, platform neutral (OCI operates under Linux Foundation)

Containers provide scalable, self healing and portable apps

## Preperation

I used a single Ubuntu 16.04.5 LTS based virtual machine on VMware ESX.

* SSH to Ubuntu virtual machine and then escalate to root

<pre><code>
vmware@ubuntu26:~$ 
vmware@ubuntu26:~$ <b>sudo -H bash</b>
root@ubuntu26:/home/vmware#
</code></pre>

*  Ensure the integrity and authenticity of the images that are downloaded from Docker Hub. GPG is based on Public Key Cryptogragphy (more info is [here](https://www.gnupg.org/))

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

* Make sure Docker is running

<pre><code>
root@ubuntu26:/home/vmware# sudo systemctl status docker
● docker.service - Docker Application Container Engine
   Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
   <b>Active: active (running)</b> since Mon 2019-06-17 12:33:25 EDT; 3min 48s ago
     Docs: https://docs.docker.com
 Main PID: 3305 (dockerd)
   CGroup: /system.slice/docker.service
           └─3305 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Jun 17 12:33:24 ubuntu26 dockerd[3305]: time="2019-06-17T12:33:24.322015384-04:00" level=warning msg="Your kernel does not support swap memory limit"
Jun 17 12:33:24 ubuntu26 dockerd[3305]: time="2019-06-17T12:33:24.322303782-04:00" level=warning msg="Your kernel does not support cgroup rt period"
Jun 17 12:33:24 ubuntu26 dockerd[3305]: time="2019-06-17T12:33:24.322529745-04:00" level=warning msg="Your kernel does not support cgroup rt runtime"
Jun 17 12:33:24 ubuntu26 dockerd[3305]: time="2019-06-17T12:33:24.323451458-04:00" level=info msg="Loading containers: start."
Jun 17 12:33:24 ubuntu26 dockerd[3305]: time="2019-06-17T12:33:24.867494590-04:00" level=info <b>msg="Default bridge (docker0) is assigned with an IP address 172.17.0.0/16. Daemon option --bip</b>
Jun 17 12:33:24 ubuntu26 dockerd[3305]: time="2019-06-17T12:33:24.944308261-04:00" level=info msg="Loading containers: done."
Jun 17 12:33:25 ubuntu26 dockerd[3305]: time="2019-06-17T12:33:25.012992693-04:00" level=info msg="Docker daemon" commit=481bc77 graphdriver(s)=overlay2 version=18.09.6
Jun 17 12:33:25 ubuntu26 dockerd[3305]: time="2019-06-17T12:33:25.013579127-04:00" level=info msg="Daemon has completed initialization"
Jun 17 12:33:25 ubuntu26 dockerd[3305]: time="2019-06-17T12:33:25.038175853-04:00" level=info msg="API listen on /var/run/docker.sock"
Jun 17 12:33:25 ubuntu26 systemd[1]: Started Docker Application Container Engine.
root@ubuntu26:/home/vmware#
</code></pre>

The first three error messages are about being able to put memory, CPU and swap limitations to containers. Means that the Kernel seems to be old for this version of Docker. More info [here](https://support.plesk.com/hc/en-us/articles/115004786934-Docker-service-shows-warnings-Your-kernel-does-not-support-cgroup-rt-runtime) and also [here](https://docs.docker.com/config/containers/resource_constraints/). It is ok as I am not going to use that feature in Docker for now.


* Check the docker version : :

<pre><code>
dtimuralp-a02:Applications dtimuralp$ <b>docker version</b>
root@ubuntu26:/home/vmware# docker version
Client:
 Version:           <b>18.09.6</b>
 API version:       1.39
 Go version:        go1.10.8
 Git commit:        481bc77
 Built:             Sat May  4 02:35:27 2019
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          <b>18.09.6</b>
  API version:      1.39 (minimum version 1.12)
  Go version:       go1.10.8
  Git commit:       481bc77
  Built:            Sat May  4 01:59:36 2019
  OS/Arch:          linux/amd64
  Experimental:     false
root@ubuntu26:/home/vmware#
</code></pre>

Docker Client ===== API Calls ===> Docker Server (Daemon) , hence there are two docker versions are shown in this output.

* Check Docker information

<pre><code>root@ubuntu26:/home/vmware# <b>docker info</b>
Containers: 0
 Running: 0
 Paused: 0
 Stopped: 0
Images: 0
Server Version: 18.09.6
Storage Driver: overlay2
 Backing Filesystem: extfs
 Supports d_type: true
 Native Overlay Diff: true
Logging Driver: json-file
Cgroup Driver: cgroupfs
Plugins:
 Volume: local
<b>Network: bridge host macvlan null overlay</b>
 Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
<b>Swarm: inactive</b>
<b>Runtimes: runc</b>
Default Runtime: runc
Init Binary: docker-init
containerd version: bb71b10fd8f58240ca47fbb579b9d1028eea7c84
runc version: 2b18fe1d885ee5083ef9f0838fee39b62d653e30
init version: fec3683
Security Options:
 apparmor
 seccomp
  Profile: default
Kernel Version: 4.4.0-131-generic
Operating System: Ubuntu 16.04.5 LTS
OSType: linux
Architecture: x86_64
CPUs: 2
Total Memory: 3.859GiB
Name: ubuntu26
ID: 6CX6:X6JC:7QVT:KKOG:ZZO7:35XC:WLVZ:EK3Z:XD7J:CFID:CRNI:7OBA
Docker Root Dir: /var/lib/docker
Debug Mode (client): false
Debug Mode (server): false
Registry: https://index.docker.io/v1/
Labels:
Experimental: false
Insecure Registries:
 127.0.0.0/8
Live Restore Enabled: false
Product License: Community Engine

<b>WARNING: No swap limit support</b>
root@ubuntu26:/home/vmware#
</code></pre>

Docker Swarm shows up as inactive cause I am goint to use a single host for now.

Runc is Docker' s container runtime which is also donated to the Open Container Project (OCP) . For more info please check [here](https://blog.docker.com/2015/06/runc/)

Docker Operations System is Docker for MAC since I installed it on my MAC.

* Check whether if any containers run by default

<pre><code>
root@ubuntu26:/home/vmware# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
root@ubuntu26:/home/vmware#
</code></pre>

No containers running by default.

* Check whether if there is any container images in the local repository

<pre><code>
root@ubuntu26:/home/vmware# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
root@ubuntu26:/home/vmware#
</code></pre>

No images in the local repository by default.

* Check which Docker networks exist by default :

<pre><code>
root@ubuntu26:/home/vmware# docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
8dc12dd156a0        <b>bridge</b>              <b>bridge</b>              <b>local</b>
f4238f561257        host                host                local
70c020e17c5e        none                null                local
root@ubuntu26:/home/vmware#

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
