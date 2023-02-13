# Short-Notes-Docker-by-TG


## Docker Complete_Notes by TG in short


# part-1 
- Basic intro, we already understand no need for notes, lab is not covered in this video_lecture

# part-2 
- Basic intro, theory notes in pdf, lab is not covered in this video_lecture

# part-3
- theory notes in pdf, lab is not covered in this video_lecture


##### -------------------------------------------------------- #####




# part-4
- how to install docker in aws ec2-instance, how to start & stop a container
- theory notes in pdf,



## Lab_notes (part-4)
- launce one ec2-instance in amazon linux



### install docker
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



### Docker All Commands from this part-4 video Lab
```bash
docker images
docker ps
docker ps -a

# pull image from docker-hub & run that image & go inside container
[root@ip-172-31-46-47 ~]# docker run -it ubuntu /bin/bash              #Noted:-here '-it' means 'interactive terminal'

# run some commands inside container, see:
root@e863d0e43982:/# ls
bin   dev  home  lib32  libx32  mnt  proc  run   srv  tmp  var
boot  etc  lib   lib64  media   opt  root  sbin  sys  usr

# see container details like os
root@e863d0e43982:/# cat /etc/os-release

# to come out from container
root@e863d0e43982:/# exit

# See all images present in your local
[root@ip-172-31-46-47 ~]# docker images

# create a container from same image (here new container is created, not same as old container)
[root@ip-172-31-46-47 ~]# docker run -it ubuntu /bin/bash
root@ee6385c28b91:/#			#-Noted:- see here is new user's id,  match this user's id with old container id,

root@ee6385c28b91:/# exit

[root@ip-172-31-46-47 ~]# docker ps

[root@ip-172-31-46-47 ~]# docker ps -a

# pull image & create container & go inside
[root@ip-172-31-46-47 ~]# docker run -it centos /bin/bash

[root@c7382dec66d9 /]# exit

[root@ip-172-31-46-47 ~]# docker images

[root@ip-172-31-46-47 ~]# docker run -it centos /bin/bash

[root@bbfab303f615 /]# cat /etc/os-release

[root@bbfab303f615 /]# exit

[root@ip-172-31-46-47 ~]# docker ps -a

# pull image from docker-hub, but get errore
[root@ip-172-31-46-47 ~]# docker pull jenkins

# pull image from docker-hub
[root@ip-172-31-46-47 ~]# docker pull jenkins/jenkins

[root@ip-172-31-46-47 ~]# docker images

# to search docker images in cli, (Noted:-you can also go docker-hub registry & search over there)
docker search ubuntu

# run a container with a name
[root@ip-172-31-46-47 ~]# docker run -it --name hamid_container ubuntu /bin/bash

root@0f14f7774d4b:/# exit

[root@ip-172-31-46-47 ~]# docker ps -a

# start a 'stop container'
[root@ip-172-31-46-47 ~]# docker start hamid_container

[root@ip-172-31-46-47 ~]# docker start intelligent_dhawan

[root@ip-172-31-46-47 ~]# docker ps

# go inside running container
[root@ip-172-31-46-47 ~]# docker attach hamid_container

root@0f14f7774d4b:/# exit       #-here container will be stoped,#

# stop a 'start container'
[root@ip-172-31-46-47 ~]# docker stop intelligent_dhawan

[root@ip-172-31-46-47 ~]# docker ps -a

# remove/delete a 'stop container'
[root@ip-172-31-46-47 ~]# docker rm hamid_container

[root@ip-172-31-46-47 ~]# docker rm intelligent_dhawan

[root@ip-172-31-46-47 ~]# docker ps -a
```



### go to docker-hub website:
- https://hub.docker.com/
- go docker-hub & search images like ubuntu, cebtos, etc,



### Basic Docker Commands Notes from 'TG theory' & 'complete-devops-notes-by-TG-pdf'
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

# To pull/download image from docker hub to local machines
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
- what is Dockerfile, docker 'diff' command
- theory notes in pdf



## Lab_notes (part-5)
- ssh amazon 'ec2-instance' where docker was installed



### in this code snippet we will do this things:
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

root@737d197f4a83:/tmp# touch hr_file

root@737d197f4a83:/tmp# exit

# to see the difference between 'base image' and changes on it(container_1), then use 'diff' command
[root@ip-172-31-46-47 ~]# docker diff container_1
C /tmp
A /tmp/hr_file
C /root
A /root/.bash_history
	#Noted:- here C=Change, A=Append, D=detection

# create image(new_image) from container(container_1)
[root@ip-172-31-46-47 ~]# docker commit container_1 new_image

[root@ip-172-31-46-47 ~]# docker images

# create container fron this newly created image(new_image)
[root@ip-172-31-46-47 ~]# docker run -it --name container_2 new_image /bin/bash

# now check files which we created in container_1
root@223435c3919b:/# ls /tmp/

root@223435c3919b:/# exit

```



### 1st Dockerfile example
- we will create Dockerfile, in 'Dockerfile' D should be in uppercase & rest are in lowercase
- next we will create image(ubuntu_image) from this Dockerfile(means build/run this Dockerfile)
- next we will create container(container_3) from this image(ubuntu_image) and also check file(/tmp/testfile)

```bash
# create Dockerfile with this details see:
[root@ip-172-31-46-47 ~]# vim Dockerfile
FROM ubuntu       #this pull image from docker-hub if locally not availabe
RUN echo "Hello world" > /tmp/testfile         #this file is created into newly created image,#

# create image from this Dockerfile 
[root@ip-172-31-46-47 ~]# docker build -t ubuntu_image .      #here '-t ubuntu_image' means give tag,#

[root@ip-172-31-46-47 ~]# docker images

# create container(container_3) from this image(ubuntu_image) and also check file(/tmp/testfile)
[root@ip-172-31-46-47 ~]# docker run -it --name container_3 ubuntu_image /bin/bash

root@b093681d16c9:/# ls /tmp/

root@b093681d16c9:/# exit

```



### 2nd Dockerfile example
- we will 1st create some files in linux host os
- we will create Dockerfile,
- next we will create image(ubuntu_image2) from this Dockerfile(means build/run this Dockerfile)
- next we will create container(container_4) from this image(ubuntu_image2) and also check file & variable

```bash
# 1st create these files in linux host os:-
touch f1.txt f2
tar -cvf f2.tar f2
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

[root@ip-172-31-46-47 ~]# docker images

# create container(container_4) from this image(ubuntu_image2) and also check files
[root@ip-172-31-46-47 ~]# docker run -it --name container_4 ubuntu_image2 /bin/bash

root@f35f6ae51d70:/tmp# ls

root@f35f6ae51d70:/tmp# echo $myname
Hamid_Raza

root@f35f6ae51d70:/tmp# exit

[root@ip-172-31-46-47 ~]# docker ps -a

```

##### ------------------------------------------- #####









# part-6
- docker volume & how to share it
- theory in pdf notes
- container ka volume mapping with another container ya with host_vm ka mtlb hai apas me synchronise/replicate hona, mtlb ek jagah jo changes hoga wo dusri jagah bhi changes hoga.



## in this part we will see 3 things about volume(docker_volume)
- 1st is- create volume by using Dockerfile & Shared that volume with another container
- 2nd is- Create volume by using commands & Shared that volume with another container
- 3rd is- Mapped container_volume with host_volume(folder/directory)



## Lab_notes (part-6)
- launce one ec2-instance in amazon linux



### install docker
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



### create volume by using Dockerfile & Shared that volume with another container
```bash
# create files in Docker_Host(Aamzon-linux)
touch file1 file2

# create Dockerfile with this details
vim Dockerfile
FROM ubuntu
VOLUME ["/myvolume"]			#this command is created volume in container,#

# create image from this Dockerfile
[root@ip-172-31-40-103 ~]# docker build -t my_image1 .

[root@ip-172-31-40-103 ~]# docker images

# create container(container_1) from this image(my_image1)
[root@ip-172-31-40-103 ~]# docker run -it --name container_1 my_image1 /bin/bash

root@b8e3572f32c1:/# ls    #here shown 'myvolume'
bin   dev  home  lib32  libx32  mnt       opt   root  sbin  sys  usr
boot  etc  lib   lib64  media   myvolume  proc  run   srv   tmp  var

# go inside 'myvolume' & create files
root@b8e3572f32c1:/# cd myvolume/
root@b8e3572f32c1:/myvolume# touch filex filey filez

root@b8e3572f32c1:/myvolume# exit

# create one more container(container_2) from image(ubuntu) & Shared that volume(myvolume) with container_2
[root@ip-172-31-40-103 ~]# docker run -it --name container_2 --privileged=true --volumes-from container_1 ubuntu /bin/bash

# check volume (myvolume) is shared with this container_2 or not
root@e0bb662ff821:/# ls

root@e0bb662ff821:/# ls myvolume/     #yes shown, it means volume is shared with this container_2

# now go inside this volume(myvolume) & create some files
root@e0bb662ff821:/# cd myvolume/

root@e0bb662ff821:/myvolume# touch f1 f2 f3

root@e0bb662ff821:/# exit

# now go to container_1, start container_1 & go inside & check volume(myvolume) data is updated or not, 
[root@ip-172-31-40-103 ~]# docker start container_1

[root@ip-172-31-40-103 ~]# docker attach container_1

root@b8e3572f32c1:/# ls myvolume/		#yes data is updated,3
f1  f2  f3  filex  filey  filez

```



### Create volume by using commands & Shared that volume with another container
```bash
# create container(container_3) from image(ubuntu) & create volume(volume2) with container_3
[root@ip-172-31-40-103 ~]# docker run -it --name container_3 -v /volume2 ubuntu /bin/bash

root@84bc97fa817d:/# ls

root@84bc97fa817d:/# cd volume2/

root@84bc97fa817d:/volume2# touch vol1 vol2 vol3

root@84bc97fa817d:/volume2# exit


# create one more container(container_4) from image(ubuntu) & Shared that volume(volume2) with container_4
[root@ip-172-31-40-103 ~]# docker run -it --name container_4 --privileged=true --volumes-from container_3 ubuntu /bin/bash

root@e1ecc91dd341:/# ls

root@e1ecc91dd341:/# cd volume2/

root@e1ecc91dd341:/volume2# ls       #shown files

root@e1ecc91dd341:/volume2# touch hamid

root@e1ecc91dd341:/volume2# exit


# now go to container_3, & start container_3 & go inside & check volume(volume2) data is updated or not,
[root@ip-172-31-40-103 ~]# docker start container_3

[root@ip-172-31-40-103 ~]# docker attach container_3

root@84bc97fa817d:/# ls volume2/      		#yes shown upadted files,#

root@84bc97fa817d:/# exit

```



### Mapped container_volume with host_volume(folder/directory)
```bash
# check host absolute path & contents
[root@ip-172-31-40-103 ~]# pwd
/root

[root@ip-172-31-40-103 ~]# ls
Dockerfile  file1  file2

# create container(host_container) with volume(volume_3) from image(ubuntu) & mapped this volume with host_os_folder
[root@ip-172-31-40-103 ~]# docker run -it --name host_container -v /root:/volume_3 --privileged=true ubuntu /bin/bash

root@8248dbcec692:/# ls

root@8248dbcec692:/# cd volume_3/

root@8248dbcec692:/volume_3# ls				#yes shown,#

root@8248dbcec692:/volume_3# touch hr1 hr2 hr3

root@8248dbcec692:/volume_3# exit

[root@ip-172-31-40-103 ~]# ls  				#yes shown,#

```


##### ------------------------------------------- #####









# part-7
- docker port mapping, restart container, exec container and docker expose, docker port expose
- theory notes is in pdf
- 



## Lab_notes (part-7)
- ssh ec2-instance



### create containers & install services/tools inside container & access that service publicly
```bash
sudo su -

# run container in daemon(background)
[root@ip-172-31-40-103 ~]# docker run -td --name techserver -p 80:80 ubuntu

/*
explanation:- here in above command '-td' means daemon/background & '-p 80:80' me 1st wala host ka port hai r 
2nd wala docker_container ka port hai mapping k liye,
*/

# check now container is running or not
[root@ip-172-31-40-103 ~]# docker ps

# to see expose port
[root@ip-172-31-40-103 ~]# docker port techserver

# go inside this container(techserver), Noted:- iss command se bhi hum container k under jaate hai- 'docker attach container_name'
[root@ip-172-31-40-103 ~]# docker exec -it techserver /bin/bash


# update & install apache2
root@26d5b295fb43:/# apt-get update -y

root@26d5b295fb43:/# apt install apache2 -y

root@26d5b295fb43:/# cd /var/www/html/

root@26d5b295fb43:/var/www/html# echo "Hello World" > index.html

root@26d5b295fb43:/var/www/html# service apache2 restart

```



### copy public ip & paste to new tab:
- yes webpage has shown,



### run one more container from jenkins image
```bash
# exit from current container
root@26d5b295fb43:/var/www/html# exit

# run one more container from jenkins image
[root@ip-172-31-40-103 ~]# docker run -td --name myjenkins -p 8080:8080 jenkins/jenkins

[root@ip-172-31-40-103 ~]# docker ps

```



### go to new tab & paste public ip with port 8080, see:
- http://3.134.112.139:8080            #-getting error, bcuz in SG port 8080 is not allow,
- go to security group & allow port 8080
- again refresh webpage(http://3.134.112.139:8080), yes now shown jenkins dashboard


##### ------------------------------------------- #####









# part-8
- what is docker-hub & how its work, create docker-hub a/c
- theory is in pdf notes



## Lab_notes (part-8)
- ssh ec2-instance


### create container, go inside container & create some files, and create image from this container:
```bash
[ec2-user@ip-172-31-40-103 ~]$ sudo su -

# create a container
[root@ip-172-31-40-103 ~]# docker run -it ubuntu /bin/bash

# create some files inside container
root@59f77a8f7550:/# mkdir dir1

root@59f77a8f7550:/# cd dir1/

root@59f77a8f7550:/dir1# touch file1 file2 file3 test1 test2 test3 xyz

root@59f77a8f7550:/# exit

[root@ip-172-31-40-103 ~]# docker ps -a

# create image from this container which we just created
[root@ip-172-31-40-103 ~]# docker commit optimistic_pare image_ubuntu

[root@ip-172-31-40-103 ~]# docker images

```



### go to docker-hub website & create one a/c, my a/c details:
- username= hra3375
- email= hra3375@gmail.com
- password=************



### go to terminal & logine with docker-hub credentials, & push image(image_ubuntu) to docker-hub-repo:
```bash
# logine with docker-hub credentials
[root@ip-172-31-40-103 ~]# docker login

# push image(image_ubuntu) from this instance to docker-hub repo
[root@ip-172-31-40-103 ~]# docker tag image_ubuntu hra3375/image_from_container
/*
explanation: docker tag image_name docker-hub-username/image_name_for_dockerhib_repo
*/

[root@ip-172-31-40-103 ~]# docker push hra3375/image_from_container

```



### go to docker-hub a/c & see image is uploaded or not
- yes image is shown in docker-hub-repo



### next launch one ec2-instance & do these, see:
- create one ec2-instance with amazon-linux
- ssh that instance & install docker & start docker engine
- pull image from docker-hub-repo & create container from this image,
- after creating container go inside & see all files which you created before uploading/push image
```bash
# see images
[root@ip-172-31-39-55 ~]# docker images
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE


# pull image from docker-hub-repo
[root@ip-172-31-39-55 ~]# docker pull hra3375/image_from_container

# check image
[root@ip-172-31-39-55 ~]# docker images

# create container from this image(hra3375/image_from_container)
[root@ip-172-31-39-55 ~]# docker run -it --name mycontainer hra3375/image_from_container /bin/bash

# check files which you created before uploading/push
root@dc1bc4bae64f:/# ls dir1/

root@dc1bc4bae64f:/# exit

```




### go to docker-hub a/c, see how to make your docker-hub_image private, see:
- go to docker-hub a/c >> click on docker-hub-repo(hra3375/image_from_container) >> manage repository >> settings >> click on 'make private' >> done.
- once your docker-hub-image will become private then you can pull that image only when you providing docker-hub a/c credential, means you have to setup your vm with credential then you will be allow to pull.



### now try to pull that image from your docker-hub-repo
```bash
# pull your private image (get error, bcuz that image is now private)
[root@ip-172-31-39-55 ~]# docker pull hra3375/image_from_container

# now that image is private so you can pull after defining credential, see:
[root@ip-172-31-39-55 ~]# docker login

# now you can pull that image
[root@ip-172-31-39-55 ~]# docker pull hra3375/image_from_container

```



### some docker cli commands:
```bash
# remove/delete all images
[root@ip-172-31-39-55 ~]# docker rmi -f $(docker images -q)

# stop all running containers
[root@ip-172-31-39-55 ~]# docker stop $(docker ps -a -q)

# delete all stopped containers
[root@ip-172-31-39-55 ~]# docker rm $(docker ps -a -q)

```



### how to make private image into public image, see:
- go to docker-hub a/c >> click on docker-hub-repo(hra3375/image_from_container) >> settings >> click on 'make public >> done.


### how to delete docker-hub-repo, see:
- go to docker-hub a/c >> click on docker-hub-repo(hra3375/image_from_container) >> settings >> click on 'Delete repository >> done.


##### ------------------------------------------- #####
