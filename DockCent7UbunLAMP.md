Install CentOS-7 on Oracle VirtualBox VM.
================
* Download the **[CentOS-7 ISO file](https://www.centos.org/download/).** Choose the DVD ISO option and any mirror link would work fine.
* Open the Oracel VM VirtualBox Manager.
* Click New >> Name: **CentOS-7** | Type: **Linux** | Version: **RedHat (64-bit)** | Memory size: **4096** | **Creating a virtual hard disk** | **VDI (VirtualBox Disk Image)** | **Dynamically allocated** | File size: **32.00 GB** | Click Create.
* Right click on the CentOS-7 and select Settings > Storage > Under Controller: IDE - select Empty > On the right hand side > Choose the Disk icon and select the downloaded ISO file from the previous step. Click OK.
* Click Start > **Install CentOS 7** > Most of the options are self explanatory > SOFTWARE SELECTION: **Server with GUI** > Everything else will be automatically detected > **Begin Installation** > Setup the User settings > **Reboot** after the installation is successfully done.<br>
Note: *Need to switch ON the wired connection every time we start the VM so that we can connect to the internet.*

----------

Install Docker on CentOS-7
================
* `hostnamectl` *Gives the information regarding the kernel. Linux Kernel version should be 3.10 or higher.*
* `yum install epel release` *Enabling the online repositories.*
* `yum update -y` *Updating all the packages available.*
* `sudo tee /etc/yum.repos.d/docker.repo <<-'EOF'`<br>`[dockerrepo]`<br>`name=Docker Repository`<br>`baseurl=https://yum.dockerproject.org/repo/main/centos/7/`<br>`enabled=1`<br>`gpgcheck=1`<br>`pgkey=https://yum.dockerproject.org/gpg`<br>`EOF` *Adding docker repository file to CentOS repository directory.*
* `yum install -y docker-engine` *Installing Docker.*
* `systemctl enable docker.service` *Enabling docker service on startup.*
* `systemctl start docker.service` *Starting the docker service.*
* `systemctl status docker.service` *Checking the docker service.*
* `docker run --rm hello-world` *Testing docker using hello-world.*

![Hello-World](https://github.com/apuroop-apz/Docker_CentOS7_Ubuntu_LAMP/blob/master/figs/1.PNG)

Note: `su - root` *This command switches to the superuser and sets up the environment like it was logged in directly. Switching to root is always advisable before firing up the docker engine.*

---------

Pulling Ubuntu image using Docker service on CentOS-7
================
* `su - root` *Switching to superuser*
* `systemctl status docker.service` *Checking the status of the docker engine.*
* `docker pull ubuntu` *Downloading the Ubuntu image from the repository.*
* `docker run -i -t ubuntu /bin/bash/` *Switching to Ubuntu by running the docker image of Ubuntu*
* `apt-get update` *If this command runs fine then we are successfully in the Ubuntu container*
* Open a new terminal, `su - root` > `docker ps` *Lists the containers that are running currently which will be Ubuntu in our case*

![Ubuntu up and running](https://github.com/apuroop-apz/Docker_CentOS7_Ubuntu_LAMP/blob/master/figs/UbuntuInsideCentOS.PNG)


--------

Install LAMP on Ubuntu using Docker service on CentOS-7 - VirtualBox VM
================
* 
* 
* 
* 
* 
