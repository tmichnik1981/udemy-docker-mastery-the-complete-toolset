# udemy-docker-mastery-the-complete-toolset

#### Links

- [Course github](https://github.com/bretfisher/udemy-docker-mastery)
- [Docker store](https://store.docker.com/)
- [Containers internals](https://www.youtube.com/watch?v=sK5i-N34im8&feature=youtu.be&list=PLBmVKD7o3L8v7Kl_XXh3KaJl9Qw2lyuFl)

#### Docker Editions

- Docker CE
- Docker EE
- 3 major types of installs
  - Direct
  - Mac/Win or legacy Docker Toolbox
  - Cloud (Docker for AWS/Azure/Google)

##### Docker CE (Community Edition)

- Free, opensource

##### Docker EE (Enterprise Edition)

- Paid
- Certified on specific platforms
- docker.com/pricing

##### Stable

- Rolls in three months of Edge features, EE supported longer

##### Edge (beta)

- Released monthly, Stable quarterly
- Gets new features first, but only supported for a month

#### Docker on Windows

- Linux containers
- Windows Containers
- Best: Docker for Windows, but win10 pro
- Win7/8/8.1 Win10 Home use Docker Toolbox
- Win Server 2016 supports Windows containers

##### Docker on Windows 10 Pro/Ent

- Use Docker for Windows from store.docker.com
- More features then just a Linux VM
- Hyper-V with tiny Linux VM for Linux Containers
- PowerShell native

##### Docker on Windows 7, 8, 10 Home

- Use Docker Toobox, not as fancy as Docker for Windows
- Download at store.docker.com
- Runs a tiny Linix VM in VirtualBox via **docker-machine**
- Uses a bash shell to make it more like Linux/Mac options
- Does not support Windows Containers

Docker on Windows Server 2016

- Windows Server 2016 supports native Windows Containers
- Docker on Windows runs on Win 2016 but not required
  - Only do this for when you run Win 2016 locally for dev/test
- No options for previous Windows Server versions
- Hyper-V can still run Linux VM just fine

#### Docker for Windows

- [Download Docker CE](https://store.docker.com/editions/community/docker-ce-desktop-windows)
- [Set-up tab completion in powershell](https://docs.docker.com/docker-for-windows/#set-up-tab-completion-in-powershell)
- [Faq](https://docs.docker.com/docker-for-windows/faqs/)
- [Cmder download](http://cmder.net/)

##### Set up tab completion in PowerShell

1. Open PowerShell as Admin

   ```powershell
   Install-Module -Scope CurrentUser posh-docker
   ```

2. Enable the module

   ```powershell
   Import-Module posh-docker
   ```

   - In case of error

     ```powershell
     Set-ExecutionPolicy RemoteSigned
     ```

   - and repeate the import

3. Create powershell profile

   ```powershell
   if (-Not (Test-Path $PROFILE)) {
       New-Item $PROFILE –Type File –Force
   }

   Add-Content $PROFILE "`nImport-Module posh-docker"
   ```

#### Installing Docker on Linux

- get.docker.com script
  - curl -sSL https://get.docker.com/ | sh
- store.docker.com

##### Docker for Linux: Setup

1. Install Docker

   ```shell
   curl -fsSL get.docker.com -o get-docker.sh
   ```

2. Add user to **docker** group

   ```shell
   sudo usermod -aG docker <user>
   ```

3. Install docker machine 

   - [install-machine](https://docs.docker.com/machine/install-machine/)

   ```shell
   curl -L https://github.com/docker/machine/releases/download/v0.13.0/docker-machine-`uname -s`-`uname -m` >/tmp/docker-machine && \
   sudo install /tmp/docker-machine /usr/local/bin/docker-machine
   ```

4. Install docker compose

   - [install docker-compose](https://docs.docker.com/compose/install/)
   - or a newer one [github compose](https://github.com/docker/compose/releases)

   ```shell
   curl -L https://github.com/docker/compose/releases/download/1.20.0-rc1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
   chmod +x /usr/local/bin/docker-compose
   ```

#### Creating and using containers

##### Checkout Docker install and config

```shell
# check version
docker version
# variables
docker info
```

##### Starting Nginx server

- image is an application we want to run
- container is an instance of that image running as a process
- You can have many containers running off the same image
- default reginstry: hub.docker.com

```shell
# running (downloading first) nginx and routing trafic to port 80
docker container run --publish 80:80 nginx

# detached mode runs docker app in the backgroud
docker container run --publish 80:80 --detach nginx
# stopping container
docker container stop <CONTAINER_ID> 
```
- run vs start - run always creates  a new one
- name - is randomly generated

```shell
# running  nginx with a name 'webhost'
docker container run --publish 80:80 --detach --name webhost nginx 
# checking logs
docker container logs webhost
# displaying of running processes of a container
docker container top webhost
# listening all containers
docker container ls -a
# removing containers 
# running containers won't be removed
docker container rm <CONTAINER_ID> <CONTAINER_ID>...
# removing with force option
docker container rm -f <CONTAINER_ID>
```

#### Manage Multiple Containers

- docs.docker.com

```shell
# run nginx
docker container run -p 80:80 -d --name webhost nginx 
# run httpd
docker container run -p 8080:80 -d --name apache httpd 
# run mysql
# -e passing environment variables 
# GENERATED ROOT PASSWORD: oboe3eVaixiocheeDeePhohdohreepa7
docker container run -p 3306:3306 -dit -e MYSQL_RANDOM_ROOT_PASSWORD=yes --name mysql mysql 


```

#### CLI Process Monitoring

```shell
# process list in one container
docker container top <CONTAINER>
# details of one container config
docker container inspect <CONTAINER>
# performance stats for all containers
docker container stats 
# GENERATED ROOT PASSWORD: caekugh9aew6thie8eiJahleeH0oo5Ie
```

#### Getting a Shell inside Containers

- [package management](https://www.digitalocean.com/community/tutorials/package-management-basics-apt-yum-dnf-pkg)

```shell
# start new container interactively
docker container run -it

# run additional command in existing container
docker container exec -it

# runs bash in a container. Container will stop if you leave a bsh
# -it allows to use terminal
# -t simulates terminal
# -i keep session to stdin
docker container run -it --name proxy nginx bash

# starting bash in a runnning container: mysql
docker container exec -it mysql bash

```

#### Docker Networks

##### Concepts

- [format command](https://docs.docker.com/config/formatting/)


- Each container connected to a private virtual network "bridge"

- Each virtual network routes through NAT firewall on hot IP

- All containers on a virt. net. talk to each other without -p

- Best practice is to create a new virt. netw. for each app (web server + db is one app)  

- By default container does not use the same ip as its host

- You can:

  - Make new virtual networks
  - Attach containers to more then one virtual network
  - Skip virtual networks and use host IP (--net=host)
  - Use different Docker network drivers to gain new abilities 

  ```shell
  # -p - publish (port)
  # format HOST:CONTAINER
  docker container run -p 80:80 --name webhost -d nginx

  # looking for NetworkSettings.IPAddress  in a json result
  docker container inspect --format '{{ .NetworkSettings.IPAddress }}' webhost
  ```



##### CLI Management

```shell
# show networks
docker network ls

# - bridge (default) network between docker and physical network
# - host special network which skips docker virtual network and connect container directly to host's network (more performance, but less secure)  
# - none - interface unattached to anything
NETWORK ID          NAME                DRIVER              SCOPE
d778f121a6d4        bridge              bridge              local
4037c811687c        host                host                local
4efb901c93bb        none                null                local

# inspect a network
docker network inspect
# create a network 
# --driver optional
docker network create --driver
# attach a network to container
docker network connect
# detach a network from container 
docker network disconnect
# running container and attaching it to my_app_nett network
docker container run --name webhost -d --network my_app_nett nginx
# connecting / disconnecting container to network
docker network connect <NETWORK_ID> <CONTAINER_ID>
docker network disconnect <NETWORK_ID> <CONTAINER_ID>

```

##### DNS

- Static IP's for talking to containers is an antipatern
- Docker defaults the hostname to the container's name, but you can also set aliases
- Default Bridge network doest not have DNS feture. You can either use `--link` or create a new network.

###### Round Robin test

1. Create a network
   - `docker network create dude`
2. Run 2 containers `elasticsearch:2` with alias **search**
   - `docker container run -d --net dude --net-alias search elasticsearch:2`
3. Run **nslookup** in **alpine** image
   - `docker container run --rm --net dude alpine nslookup search`
4. Run curl in centos image
   - `docker container run --rm --net dude centos curl -s search:9200`

#### Container images

- [Image docs](https://github.com/moby/moby/blob/master/image/spec/v1.md)
- Image - app binaries and dependencies and metadata about the image data and how to run the image
- not a complete OS, no kernel, no kernel modules 
- [Official images](https://github.com/docker-library/official-images/tree/master/library)

##### Docker HUB

- image might has one or more tags
- "latest" - the newest software
- on production always specify the exact version
- each line in Docker Hub refers to one version

##### Image layers

- display history of an image nginx
  - `docker history nginx:latest`
- every change made on an image is a layer
- each layer has a unique SHA
- layers are shared and reused
- running a container creates new container layer
- changing files in an image from a container will cause coping those changed files to container layer 
- display metadata about the image
  - docker image inspect nginx

##### Image Tagging and pushing

- latest tag - is the default tag but image owners should assign it to the newest stable version
- tagging
  - `docker image tag <SOURCE_IMAGE[:TAG]> <TARGET_IMAGE[:TAG]>`
- pushing - uploads changed layers to a image registry
  - `docker image push brefisher/nginx`
- logging in Hub
  - `docker login <SERVER>`
- logging out
  - `docker logout`

#### Building images

- [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)

