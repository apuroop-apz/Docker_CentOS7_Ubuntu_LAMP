# Independent Study Report on Docker

### Apuroop Naidu Sadhanala
### Professor: Dr. Feng George Yu
#### Department of Computer Science and Information Systems, Youngstown State University, USA
##### Spring 2017

Docker is the latest technology that has been gaining a lot of market among the information technology (IT) professionals across the planet lately. Docker is an open source containerization engine, which automates the packaging, shipping, and deployment of any software applications that are presented as lightweight, portable, and self-sufficient containers, that will run virtually anywhere.

A docker container is a software pouch which includes everything necessary to run the software independently. There can be ‘n’ no. of Docker containers in a single machine and containers are completely isolated from each other as well as the host machine (Virtual Machine or Actual Host). Containers eliminate the friction that comes with shipping code to distant locations.

The Docker primarily consists of the Docker engine and the Docker Hub. The Docker engine is for enabling the realization of purpose-specific as well as generic Docker containers. The Docker Hub is a fast-growing repository of Docker images that is available to the programmers and developers all around the globe in a very easy manner.

The Docker is a light weight Virtual Machine which is limited to certain specifications. The outstanding business and technical advantages are the bare metal-scale performance, real-time scalability, higher availability, and many more. Most of the unwanted add-ons are eliminated to speed-up the roll-out of hundreds of application containers in seconds and to reduce the time taken for marketing and valuing in a cost-effective fashion.

The differences between a VM and a Container are as follows:

| Virtual Machines | Containers |
|-------|:---:|
| Represents hardware-level virtualization | Represents operating system virtualization |
| Heavyweight | Lightweight  |
| Slow provisioning  | Real-time provisioning and scalability   |
| Limited performance |	Native performance |
| Fully isolated and hence more secure | Process-level isolation and hence less secure |

The following figure gives a detailed understanding of Virtual Machines and Docker Containers.
![Differences](https://github.com/apuroop-apz/Docker_CentOS7_Ubuntu_LAMP/blob/master/figs/7.jpg)

# Installation Docker on CentOS-7
* `hostnamectl` *Gives the information regarding the kernel. Linux Kernel version should be 3.10 or higher.*
* `yum install epel release` *Enabling the online repositories.*
* `yum update -y` *Updating all the packages available.*
* `sudo tee /etc/yum.repos.d/docker.repo <<-'EOF'`<br>`[dockerrepo]`<br>`name=Docker Repository`<br>`baseurl=https://yum.dockerproject.org/repo/main/centos/7/`<br>`enabled=1`<br>`gpgcheck=1`<br>`pgkey=https://yum.dockerproject.org/gpg`<br>`EOF` *Adding docker repository file to CentOS repository directory.*
* `yum install -y docker-engine` *Installing Docker.*
* `systemctl enable docker.service` *Enabling docker service on startup.*
* `systemctl start docker.service` *Starting the docker service.*
* `systemctl status docker.service` *Checking the docker service.*
* `docker run --rm hello-world` *Testing docker using hello-world.*

![Hello_World](https://github.com/apuroop-apz/Docker_CentOS7_Ubuntu_LAMP/blob/master/figs/1.PNG)

###### Note: `su - root` *This command switches to the superuser and sets up the environment like it was logged in directly. Switching to root is always advisable before firing up the docker engine.*
---------

# Pulling an Ubuntu image on CentOS-7
* `su - root` *Switching to superuser*
* `systemctl status docker.service` *Checking the status of the docker engine.*
* `docker pull ubuntu` *Downloading the Ubuntu image from the repository.*
* `docker run -i -t ubuntu /bin/bash/` *Switching to Ubuntu by running the docker image of Ubuntu*
* `apt-get update` *If this command runs fine then we are successfully in the Ubuntu container*
* Open a new terminal, `su - root` > `docker ps` *Lists the containers that are running currently which will be Ubuntu in our case*

![Installation_of_Docker](https://github.com/apuroop-apz/Docker_CentOS7_Ubuntu_LAMP/blob/master/figs/UbuntuInsideCentOS.PNG)

--------


# Installation of LAMP stack on Ubuntu server as CentOS-7 host

* `docker run -i -t ubuntu /bin/bash` *Switching to the UBUNTU server*
* `apt-get update` *Updating the server*
* `apt-get install apache2` *Installing the Apache*
* `apt-get install mysql-server mysql-client` *Installing the MySQL*
* `apt-cache search php7` *Searching for the PHP 7.x version*
* `apt-get install php7.0 php7.0-mysql php7.0-curl php7.0-gd php7.0-mbstring php-gettext php7.0-mcrypt php7.0-xmlrpc libapache2-mod-php7.0` *Installing the PHP7.0*
* `exit` *Logging out*
* `docker ps -a` *Listing all the containers that are running*
* `docker commit CONTAINERID MYIMAGENAME` *Changing the Container settings using the commit command*
* `docker images` *Lists all the images. In this case, we can find the newly created LAMP image*
* `docker run -i -t -p 80:80 MYIMAGENAME /bin/bash`-p 80:80 makes the port 80 from the container bind to the port 80 of the host*
* `service apache2 start` *Starting the Apache service*
* `hostname -I` *Gives the IP address*
* Open a browser and in the address bar type your IP address

![Apache_It_Works](https://github.com/apuroop-apz/Docker_CentOS7_Ubuntu_LAMP/blob/master/figs/FINAL_Apache_ItWorks.PNG)


# Creating a LAMP stack on Ubuntu docker file and run the image

* `yum update` Updating the server
* `systemctl start docker.service` Starting the docker service 
* `systemctl status docker.service` Checking the status 
* `mkdir dockerfiles` creating a folder
* `cd dockerfiles`
* `vi lampUbuntu` Creating a dockerfile and opening it in terminal for editing
* Press "i" to type or insert content

`FROM ubuntu:14.04`<br>
`MAINTAINER Apuroop Naidu`

`Setup environment`<br>
`ENV DEBIAN_FRONTEND noninteractive`

`#Update sources`<br>
`RUN apt-get update -y`


`#Install apache`<br>
`RUN apt-get install -y apache2 vim bash-completion unzip`<br>
`RUN mkdir -p /var/lock/apache2 /var/run/apache2`


`#Install mysql`<br>
`RUN apt-get install -y mysql-client mysql-server`<br>
`RUN echo "NETWORKING=yes" > /etc/sysconfig/network`

`#Start mysql to create initial tables`<br>
`RUN service mysql start`


`#Install php`<br>
`RUN apt-get install -y php7.0 php7.0-mysql php7.0-dev php7.0-gd php7.0-pspell php7.0-snmp snmp php7.0-xmlrpc libapache2-mod-php7.0 php7.0-cli`


`#Install supervisord`<br>
`RUN apt-get install -y supervisor`<br>
`RUN mkdir -p /var/log/supervisor`


`#Install sshd`<br>
`RUN apt-get install -y openssh-server openssh-client passwd`<br>
`RUN mkdir -p /var/run/sshd`


`RUN ssh-keygen -q -N "" -t dsa -f /etc/ssh/ssh_host_dsa_key && ssh-keygen -q -N "" -t rsa -f /etc/ssh/ssh_host_rsa_key`<br>
`RUN sed -ri 's/PermitRootLogin without-password/PermitRootLogin yes/g' /etc/ssh/sshd_config`<br>
`RUN echo 'root:changeme' | chpasswd`


`#Put your own public key at id_rsa.pub for key-based login.`<br>
`RUN mkdir -p /root/.ssh && touch /root/.ssh/authorized_keys && chmod 700 /root/.ssh`<br>
`ADD id_rsa.pub /root/.ssh/authorized_keys`


`ADD phpinfo.php /var/www/html/`<br>
`ADD supervisord.conf /etc/`<br>
`EXPOSE 22 80 443`

`CMD ["supervisord", "-n"]`

* Press Esc and type wq! to save and exit the editor in terminal.
* `docker build lampUbuntu .` Building the docker. This should be done by staying in the folder the dockerfile is in.

# Useful commands

* `sudo docker ps -a` Lists all the containers and gives the status too.
* `sudo docker top` Lists the running processes of a container.
* `sudo docker pause` This will pause the container and its processes
* `sudo docker logs -t` Gives the timestamp
* `sudo docker images` Lists the images that are pulled and created as well.
* `ifconfig` Lists the detailed information
* `systemctl status docker.service` Gives the status of the docker engine
