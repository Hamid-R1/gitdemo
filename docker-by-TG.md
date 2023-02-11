# docker-by-TG


# part-1 
- Basic intro, we already understand no need for notes, lab is not covered in this video_lecture

# part-2 
- Basic intro, we already understand no need for notes, lab is not covered in this video_lecture

# part-3
- theory notes in pdf, lab is not covered in this video_lecture



# part-4
- theory notes in pdf, Lab notes is in below



# Lab_notes (part-4)
- launce one ec2-instance in amazon linux


## install docker
```bash
sudo su -

# update & install docker
yum update -y
yum install docker -y
which docker
docker -v      or    docker --version

systemctl status docker
systemctl start docker
systemctl enable docker

docker info      #to see more details about docker & host os
```





## Docker All Commands from this part-4 video Lab
```bash
docker images
docker ps
docker ps -a

# pull image from docker-hub & run that image & go inside container
[root@ip-172-31-46-47 ~]# docker run -it ubuntu /bin/bash              #Noted:-here '-it' means 'interactive terminal'
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
677076032cca: Pull complete
Digest: sha256:9a0bdde4188b896a372804be2384015e90e3f84906b750c1a53539b585fbbe7f
Status: Downloaded newer image for ubuntu:latest
root@e863d0e43982:/#			#-here see you are inside container


# run some commands inside container, see:
root@e863d0e43982:/# ls
bin   dev  home  lib32  libx32  mnt  proc  run   srv  tmp  var
boot  etc  lib   lib64  media   opt  root  sbin  sys  usr


# see container details like os
root@e863d0e43982:/# cat /etc/os-release
PRETTY_NAME="Ubuntu 22.04.1 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.1 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy


# to come out from container
root@e863d0e43982:/# exit
exit
[root@ip-172-31-46-47 ~]#


# See all images present in your local
[root@ip-172-31-46-47 ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
ubuntu       latest    58db3edaf2be   2 weeks ago   77.8MB


# create a container from same image (here new container is created, not same as old container)
[root@ip-172-31-46-47 ~]# docker run -it ubuntu /bin/bash
root@ee6385c28b91:/#			#-Noted:- see here is new user's id,  match this user's id with old container id,

root@ee6385c28b91:/# exit
exit
[root@ip-172-31-46-47 ~]#


[root@ip-172-31-46-47 ~]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES


[root@ip-172-31-46-47 ~]# docker ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS                      PORTS     NAMES
ee6385c28b91   ubuntu    "/bin/bash"   4 minutes ago    Exited (0) 58 seconds ago             intelligent_dhawan
e863d0e43982   ubuntu    "/bin/bash"   16 minutes ago   Exited (0) 9 minutes ago              eager_chaum


# pull image & create container & go inside
[root@ip-172-31-46-47 ~]# docker run -it centos /bin/bash
Unable to find image 'centos:latest' locally
latest: Pulling from library/centos
a1d0c7532777: Pull complete
Digest: sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177
Status: Downloaded newer image for centos:latest
[root@c7382dec66d9 /]#

[root@c7382dec66d9 /]# exit
exit
[root@ip-172-31-46-47 ~]#


[root@ip-172-31-46-47 ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
ubuntu       latest    58db3edaf2be   2 weeks ago     77.8MB
centos       latest    5d0da3dc9764   17 months ago   231MB


[root@ip-172-31-46-47 ~]# docker run -it centos /bin/bash
[root@bbfab303f615 /]#


[root@bbfab303f615 /]# cat /etc/os-release
NAME="CentOS Linux"
VERSION="8"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="8"
PLATFORM_ID="platform:el8"
PRETTY_NAME="CentOS Linux 8"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:8"
HOME_URL="https://centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"
CENTOS_MANTISBT_PROJECT="CentOS-8"
CENTOS_MANTISBT_PROJECT_VERSION="8"


[root@bbfab303f615 /]# exit
exit
[root@ip-172-31-46-47 ~]#


[root@ip-172-31-46-47 ~]# docker ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED              STATUS                      PORTS     NAMES
bbfab303f615   centos    "/bin/bash"   About a minute ago   Exited (0) 14 seconds ago             blissful_hofstadter
c7382dec66d9   centos    "/bin/bash"   4 minutes ago        Exited (0) 3 minutes ago              jolly_easley
ee6385c28b91   ubuntu    "/bin/bash"   13 minutes ago       Exited (0) 10 minutes ago             intelligent_dhawan
e863d0e43982   ubuntu    "/bin/bash"   25 minutes ago       Exited (0) 18 minutes ago             eager_chaum



# pull image from docker-hub, but get errore
[root@ip-172-31-46-47 ~]# docker pull jenkins
Using default tag: latest
Error response from daemon: manifest for jenkins:latest not found: manifest unknown: manifest unknown


# pull image from docker-hub
[root@ip-172-31-46-47 ~]# docker pull jenkins/jenkins
Using default tag: latest
latest: Pulling from jenkins/jenkins
699c8a97647f: Pull complete
656837bc63c3: Pull complete
81ba85001557: Pull complete
f565cdb160fe: Pull complete
7f0db80857b0: Pull complete
b51e09c8a0bd: Pull complete
a02b1ab95401: Pull complete
b113c3f8acf6: Pull complete
90c616f07a2d: Pull complete
22b926230283: Pull complete
32fdd8eaf030: Pull complete
e04af8aa1a05: Pull complete
3938fe646b55: Pull complete
Digest: sha256:8656eb80548f7d9c7be5d1f4c367ef432f2dd62f81efa86795c9155258010d99
Status: Downloaded newer image for jenkins/jenkins:latest
docker.io/jenkins/jenkins:latest


[root@ip-172-31-46-47 ~]# docker images
REPOSITORY        TAG       IMAGE ID       CREATED         SIZE
jenkins/jenkins   latest    f16216f97fcb   2 days ago      467MB
ubuntu            latest    58db3edaf2be   2 weeks ago     77.8MB
centos            latest    5d0da3dc9764   17 months ago   231MB



# to search docker images in cli, (Noted:-you can also go docker-hub registry & search over there)
docker search ubuntu


# run a container with a name
[root@ip-172-31-46-47 ~]# docker run -it --name hamid_container ubuntu /bin/bash
root@0f14f7774d4b:/# exit
exit
[root@ip-172-31-46-47 ~]#



[root@ip-172-31-46-47 ~]# docker ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED              STATUS                           PORTS     NAMES
0f14f7774d4b   ubuntu    "/bin/bash"   About a minute ago   Exited (0) About a m inute ago             hamid_container
bbfab303f615   centos    "/bin/bash"   21 minutes ago       Exited (0) 20 minute s ago                 blissful_hofstadter
c7382dec66d9   centos    "/bin/bash"   24 minutes ago       Exited (0) 23 minute s ago                 jolly_easley
ee6385c28b91   ubuntu    "/bin/bash"   34 minutes ago       Exited (0) 30 minute s ago                 intelligent_dhawan
e863d0e43982   ubuntu    "/bin/bash"   45 minutes ago       Exited (0) 38 minute s ago                 eager_chaum


# start a 'stop container'
[root@ip-172-31-46-47 ~]# docker start hamid_container
hamid_container

[root@ip-172-31-46-47 ~]# docker start intelligent_dhawan
intelligent_dhawan


[root@ip-172-31-46-47 ~]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS          PORTS     NAMES
0f14f7774d4b   ubuntu    "/bin/bash"   6 minutes ago    Up 3 minutes              hamid_container
ee6385c28b91   ubuntu    "/bin/bash"   39 minutes ago   Up 15 seconds             intelligent_dhawan


# go inside running container
[root@ip-172-31-46-47 ~]# docker attach hamid_container
root@0f14f7774d4b:/#

root@0f14f7774d4b:/# exit       #-here container will be stoped,#
exit
[root@ip-172-31-46-47 ~]#


# stop a 'start container'
[root@ip-172-31-46-47 ~]# docker stop intelligent_dhawan
intelligent_dhawan


[root@ip-172-31-46-47 ~]# docker ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS                            PORTS     NAMES
0f14f7774d4b   ubuntu    "/bin/bash"   13 minutes ago   Exited (0) 4 minutes ago                    hamid_container
bbfab303f615   centos    "/bin/bash"   33 minutes ago   Exited (0) 32 minutes ago                   blissful_hofstadter
c7382dec66d9   centos    "/bin/bash"   36 minutes ago   Exited (0) 35 minutes ago                   jolly_easley
ee6385c28b91   ubuntu    "/bin/bash"   46 minutes ago   Exited (137) About a minute ago             intelligent_dhawan
e863d0e43982   ubuntu    "/bin/bash"   58 minutes ago   Exited (0) 50 minutes ago                   eager_chaum


# remove/delete a 'stop container'
[root@ip-172-31-46-47 ~]# docker rm hamid_container
hamid_container

[root@ip-172-31-46-47 ~]# docker rm intelligent_dhawan
intelligent_dhawan


[root@ip-172-31-46-47 ~]# docker ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED             STATUS                      PORTS     NAMES
bbfab303f615   centos    "/bin/bash"   37 minutes ago      Exited (0) 36 minutes ago             blissful_hofstadter
c7382dec66d9   centos    "/bin/bash"   40 minutes ago      Exited (0) 39 minutes ago             jolly_easley
e863d0e43982   ubuntu    "/bin/bash"   About an hour ago   Exited (0) 54 minutes ago             eager_chaum

```



## go to docker-hub website & create your a/c:
- create one a/c (not created as of now)





## Basic Docker Commands Notes from 'TG theory' & 'complete-devops-notes-by-TG-pdf'
```bash 		
# install docker
yum install docker -y

# start, enable, status
systemctl status docker
systemctl start docker
systemctl enable docker

# uninstall docker
remove docker -y

# See all images present in your local
docker images   or docker image ls

# To find out images in Docker hub (image_name must be matching from docker-hub)
docker search image_name

# To download image from docker hub to local machines
docker pull image_name


# To create a container
docker run -it ubuntu /bin/bash

# To create a container with name
docker run -it --name hamid_container ubuntu /bin/bash

# To start container
docker start container_name

# To go inside container
docker attach container_name

# To see all containers
docker ps -a 

# to see only running containers (PS=Process Status)
docker ps

# To check the network status inside container
docker network inspect container_name

# To running a container in the background
docker run -d container_name

# To stop container (always stop container before delete)
docker stop container_name

# to remove container
docker rm container_name

# to remove running container
docker rm -f container_name

# To remove all containers
docker container_name prune 
```

##### -------------------------------------------------------- #####






# part-5
- theory notes in screenshot and in pdf


# Lab_notes (part-5)
- ssh amazon 'ec2-instance' where docker was installed



## in this code snippet we will do this things:
- create one container(container_1) from base image(ubuntu)
- in container_1 we create one file & exit from container_1
- next we will see the difference between 'base image' and changes on it(container_1) byusing 'diff' command
- next we will create image(new_image) from container(container_1)
- next we wil create container(container_2) fron this newly created image(new_image)
- next we will check files which we created in container_1
```bash
sudo su -

# create one container & go inside
docker run --name container_1 -it ubuntu /bin/bash


# do this inside container
root@737d197f4a83:/# ls
bin   dev  home  lib32  libx32  mnt  proc  run   srv  tmp  var
boot  etc  lib   lib64  media   opt  root  sbin  sys  usr

root@737d197f4a83:/# cd tmp/
root@737d197f4a83:/tmp#

root@737d197f4a83:/tmp# touch hr_file

root@737d197f4a83:/tmp# ls
hr_file

root@737d197f4a83:/tmp# exit
exit
[root@ip-172-31-46-47 ~]#


# to see the difference between 'base image' and changes on it(container_1), then use 'diff' command
[root@ip-172-31-46-47 ~]# docker diff container_1
C /tmp
A /tmp/hr_file
C /root
A /root/.bash_history
	#Noted:- here C=Change, A=Append, D=detection


# create image(new_image) from container(container_1)
[root@ip-172-31-46-47 ~]# docker commit container_1 new_image
sha256:38800e4b2504df1b9503cb56e38a6c00600c88ecc036ba3a073eac83188f1caa


[root@ip-172-31-46-47 ~]# docker images
REPOSITORY        TAG       IMAGE ID       CREATED          SIZE
new_image         latest    38800e4b2504   29 seconds ago   77.8MB
jenkins/jenkins   latest    f16216f97fcb   2 days ago       467MB
ubuntu            latest    58db3edaf2be   2 weeks ago      77.8MB
centos            latest    5d0da3dc9764   17 months ago    231MB


# create container fron this newly created image(new_image)
[root@ip-172-31-46-47 ~]# docker run -it --name container_2 new_image /bin/bash
root@223435c3919b:/#

# now check files which we created in container_1
root@223435c3919b:/# ls /tmp/
hr_file

root@223435c3919b:/# exit
exit
[root@ip-172-31-46-47 ~]#

```






## 1st Dockerfile example
- we will create Dockerfile, D should be in uppercase & rest are in lowercase
- next we will create image(ubuntu_image) from this Dockerfile(means build/run this Dockerfile)
- next we will create container(container_3) from this image(ubuntu_image) and also check file(/tmp/testfile)
```bash
# create Dockerfile with this details see:
[root@ip-172-31-46-47 ~]# vim Dockerfile
FROM ubuntu       #this pull image from docker-hub if locally not availabe
RUN echo "Hello world" > /tmp/testfile         #this file is created into newly created image,#


# create image from this Dockerfile 
[root@ip-172-31-46-47 ~]# docker build -t ubuntu_image .      #here '-t ubuntu_image' means give tag,#
Sending build context to Docker daemon   12.8kB
Step 1/2 : FROM ubuntu
 ---> 58db3edaf2be
Step 2/2 : RUN echo "Hello world" > /tmp/testfile
 ---> Running in d45c15ef415d
Removing intermediate container d45c15ef415d
 ---> 2f46ca0dfce2
Successfully built 2f46ca0dfce2
Successfully tagged ubuntu_image:latest


[root@ip-172-31-46-47 ~]# docker images
REPOSITORY        TAG       IMAGE ID       CREATED          SIZE
ubuntu_image      latest    2f46ca0dfce2   9 seconds ago    77.8MB
new_image         latest    38800e4b2504   53 minutes ago   77.8MB
jenkins/jenkins   latest    f16216f97fcb   2 days ago       467MB
ubuntu            latest    58db3edaf2be   2 weeks ago      77.8MB
centos            latest    5d0da3dc9764   17 months ago    231MB


# create container(container_3) from this image(ubuntu_image) and also check file(/tmp/testfile)
[root@ip-172-31-46-47 ~]# docker run -it --name container_3 ubuntu_image /bin/bash
root@b093681d16c9:/#

root@b093681d16c9:/# ls /tmp/
testfile

root@b093681d16c9:/# cat /tmp/testfile
Hello world

root@b093681d16c9:/# exit
exit
[root@ip-172-31-46-47 ~]#

```






## 2nd Dockerfile example
- we will 1st create some files in linux host os
- we will create Dockerfile,
- next we will create image(ubuntu_image2) from this Dockerfile(means build/run this Dockerfile)
- next we will create container(container_4) from this image(ubuntu_image2) and also check file & variable
```bash
# 1st create these files in linux host os:-
touch f1.txt
touch f2
tar -cvf f2.tar f2
ls
Dockerfile  f1.txt  f2  f2.tar

gzip f2.tar
ls
Dockerfile  f1.txt  f2  f2.tar.gz

rm -rf f2
ls
Dockerfile  f1.txt  f2.tar.gz


# create Dockerfile with this details see:
[root@ip-172-31-46-47 ~]# vim Dockerfile
FROM ubuntu
WORKDIR /tmp		#this working directory for base_image,#
RUN echo "Good morning" > /tmp/file_1			#this file is created into newly created image,#
ENV myname Hamid_Raza				# this is variable, here variable is 'myname' & value is Hamid_Raza.#
COPY f1.txt /tmp					# copy from host os to this base_image
ADD f2.tar.gz /tmp					# copy & unzip this tar file from host os to this base_image


# create image from this Dockerfile
[root@ip-172-31-46-47 ~]# docker build -t ubuntu_image2 .
Sending build context to Docker daemon  14.85kB
Step 1/6 : FROM ubuntu
 ---> 58db3edaf2be
Step 2/6 : WORKDIR /tmp
 ---> Running in 8d5e4bd98d89
Removing intermediate container 8d5e4bd98d89
 ---> b44a302658ae
Step 3/6 : RUN echo "Good morning" > /tmp/file_1
 ---> Running in 60b321af6885
Removing intermediate container 60b321af6885
 ---> 52f0e59f7cd4
Step 4/6 : ENV myname Hamid_Raza
 ---> Running in 9d34c889ba61
Removing intermediate container 9d34c889ba61
 ---> 6665543cee25
Step 5/6 : COPY f1.txt /tmp
 ---> ac29c62efdd8
Step 6/6 : ADD f2.tar.gz /tmp
 ---> 8a4369de8e28
Successfully built 8a4369de8e28
Successfully tagged ubuntu_image2:latest


[root@ip-172-31-46-47 ~]# docker images
REPOSITORY        TAG       IMAGE ID       CREATED              SIZE
ubuntu_image2     latest    8a4369de8e28   About a minute ago   77.8MB
ubuntu_image      latest    2f46ca0dfce2   52 minutes ago       77.8MB
new_image         latest    38800e4b2504   2 hours ago          77.8MB
jenkins/jenkins   latest    f16216f97fcb   2 days ago           467MB
ubuntu            latest    58db3edaf2be   2 weeks ago          77.8MB
centos            latest    5d0da3dc9764   17 months ago        231MB


# create container(container_4) from this image(ubuntu_image2) and also check files
[root@ip-172-31-46-47 ~]# docker run -it --name container_4 ubuntu_image2 /bin/bash
root@f35f6ae51d70:/tmp#

root@f35f6ae51d70:/tmp# ls
f1.txt  f2  file_1

root@f35f6ae51d70:/tmp# echo $myname
Hamid_Raza

root@f35f6ae51d70:/tmp# exit
exit
[root@ip-172-31-46-47 ~]#


[root@ip-172-31-46-47 ~]# docker ps -a
CONTAINER ID   IMAGE           COMMAND       CREATED          STATUS                      PORTS     NAMES
f35f6ae51d70   ubuntu_image2   "/bin/bash"   10 minutes ago   Exited (0) 25 seconds ago             container_4
b093681d16c9   ubuntu_image    "/bin/bash"   57 minutes ago   Exited (0) 53 minutes ago             container_3
223435c3919b   new_image       "/bin/bash"   2 hours ago      Exited (0) 2 hours ago                container_2
737d197f4a83   ubuntu          "/bin/bash"   2 hours ago      Exited (0) 2 hours ago                container_1
dca5e4a165a4   ubuntu          "/bin/bash"   4 hours ago      Exited (1) 4 hours ago                hamid_container1
27a714f6f608   ubuntu          "/bin/bash"   4 hours ago      Exited (0) 4 hours ago                hamid_container
bbfab303f615   centos          "/bin/bash"   6 hours ago      Exited (0) 6 hours ago                blissful_hofstadter
c7382dec66d9   centos          "/bin/bash"   6 hours ago      Exited (0) 6 hours ago                jolly_easley
e863d0e43982   ubuntu          "/bin/bash"   6 hours ago      Exited (0) 6 hours ago                eager_chaum
```

##### ------------------------------------------- #####







# part-6
- docker volume & how to share it
- theory in pdf notes



# Lab_notes (part-6)
- launce one ec2-instance in amazon linux


## install docker
```bash
sudo su -

# update & install docker
yum update -y
yum install docker -y
which docker
docker -v      or    docker --version

systemctl status docker
systemctl start docker
systemctl enable docker

docker info      #to see more details about docker & host os
```




## create volume by using Dockerfile & Shared that volume with another container
```bash
# create files in Docker_Host(Aamzon-linux)
touch file1 file2


# create Dockerfile with this details
vim Dockerfile
FROM ubuntu
VOLUME ["/myvolume"]


# create image from this Dockerfile
[root@ip-172-31-40-103 ~]# docker build -t my_image1 .
Sending build context to Docker daemon  11.78kB
Step 1/2 : FROM ubuntu
latest: Pulling from library/ubuntu
677076032cca: Pull complete
Digest: sha256:9a0bdde4188b896a372804be2384015e90e3f84906b750c1a53539b585fbbe7f
Status: Downloaded newer image for ubuntu:latest
 ---> 58db3edaf2be
Step 2/2 : VOLUME ["/myvolume"]
 ---> Running in 0f9149eace3d
Removing intermediate container 0f9149eace3d
 ---> 3e66b0449930
Successfully built 3e66b0449930
Successfully tagged my_image1:latest


[root@ip-172-31-40-103 ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
my_image1    latest    3e66b0449930   54 seconds ago   77.8MB
ubuntu       latest    58db3edaf2be   2 weeks ago      77.8MB


# create container(container_1) from this image(my_image1)
[root@ip-172-31-40-103 ~]# docker run -it --name container_1 my_image1 /bin/bash
root@b8e3572f32c1:/#


root@b8e3572f32c1:/# ls    #here shwn 'myvolume'
bin   dev  home  lib32  libx32  mnt       opt   root  sbin  sys  usr
boot  etc  lib   lib64  media   myvolume  proc  run   srv   tmp  var


# go inside 'myvolume' & create files
root@b8e3572f32c1:/# cd myvolume/
root@b8e3572f32c1:/myvolume# touch filex filey filez

root@b8e3572f32c1:/myvolume# exit
exit
[root@ip-172-31-40-103 ~]#



# create one more container(container_2) from image(ubuntu)
[root@ip-172-31-40-103 ~]# docker run -it --name container_2 --privileged=true --volumes-from container_1 ubuntu /bin/bash
root@e0bb662ff821:/#


# check volume (myvolume) is shared with this container_2 or not
root@e0bb662ff821:/# ls
bin   dev  home  lib32  libx32  mnt       opt   root  sbin  sys  usr
boot  etc  lib   lib64  media   myvolume  proc  run   srv   tmp  var

root@e0bb662ff821:/# ls myvolume/     #yes shown, it means volume is shared with this container_2
filex  filey  filez

# now go inside this volume(myvolume) & create some files
root@e0bb662ff821:/# cd myvolume/

root@e0bb662ff821:/myvolume# touch f1 f2 f3

root@e0bb662ff821:/myvolume# ls
f1  f2  f3  filex  filey  filez

root@e0bb662ff821:/# exit
exit
[root@ip-172-31-40-103 ~]#


# now go to container_1 & check volume(myvolume) data is updated or not, start container_1 & go inside
[root@ip-172-31-40-103 ~]# docker start container_1
container_1

[root@ip-172-31-40-103 ~]# docker attach container_1
root@b8e3572f32c1:/#

root@b8e3572f32c1:/# ls myvolume/		#yes data is updated,3
f1  f2  f3  filex  filey  filez

```




-----37:00...............
## Create volume by using command
```bash


```

