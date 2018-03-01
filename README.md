# udemy-docker-mastery-the-complete-toolset

#### Links

- [Course github](https://github.com/bretfisher/udemy-docker-mastery)
- [Docker store](https://store.docker.com/)

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

#### Docker Toolbox for Windows

