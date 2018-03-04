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

