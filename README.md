#docker 

we are using for containeraizing the applications. so once developers wrote the code we will sit with developers and need to know about what dependencies need for application along with the system dependencies.

container is nothing but the packaging the application along with the system dependencies and application dependencies.

docker also have some defaluts 
docker deamoan acts as a root level if the any container compromize security and accesed by the hacker. The heacker can able hack entire docker host.
docker deamon is a single point failure if the docker host goes down entire containers also goes down.

for the security we have to use detroless images these images even dont have ls, curl. These images only have a application runtime.


normally we are using containers instead of virtual machines for reduceing the size, but virtual mchines are more securable compared to the containers.

for the reduceing the security oundarabulitys we have to use multi stage bulids for create the container along with we have to use destroless images. before only i discussed about the destroless images doesnot have anything execept the application runtime.

- If we use go language we dont need application runtime so we can use scratch. because it is the    very low size image.

- If we use python language we have to use pythonruntime destroless images

- same If we use java language we have to use javalanguage destroless images


first we have to install docker client is nothing but the docker cli to perform the docker commands

its also installed the docker deamoen this main work is taking the commands from the docker client and exicute the commands.

we aill see how to write the dockerfile. the docker file name compulsory on the name of dockerfile

DOCKERFILE

FROM baseimage eg: nginx, ubuntu,
COPY . ./app
ADD . ./app alog this we can copy the data from any urls Ex: s3 bucket
WORKDIR      # where you want to work
RUN apt update && apt-get install git
CMD             # it will append to the entrypoint 
ENTRYPOINT      # after append it will exicute first command after run the container.

# most importtant commands for docker

- to check the docker installed or noe
# docker
- to check the docker version
# docker -v
- once you wrote the docker file for buliding the image
# docker build -t <imagename> . 0r path of the dockerfile
- show the images
# docker images     
- to create the containers
# docker create -- name <name if the container> imagename (or) imageid
- to run the containers
# docker run -p 8080:80 -d -it containername

here -p exposing the ports to access the container 8080 is the host post 80 is the container port 
here -d is the detach mode it will run on beckground
-it is the interactive mode to ietract the container

- do you run image as a container and also you can interact the container and perform the activities inside of the container.
# docker run  exec -it --name containername -p 8080:80 <imagename>   

- to show the running containers
# docker ps

- to show the all the containers 
# docker ps -a

- to start the container
# docker start container name

- to stop the container
# docker stop containername

- to restart the container
# docker restart containername

- to remove the container
# docker rm containerid or containername
- to remove all containers at a time
# docker conatiner prune

- to remove the images
# docker rmi imagename
- to remove all conatiners at a time
# docker image prune (sometimes we have to use) -a

networks # importtant

-- bridge
-- host 
-- none
# bridge network is the default network by using port mapping we can access the containers. if you attaches same bridge network to multible containers the containers can able the communicate by using virtual ethernetnet cable.
# we can also creatw multible bridge networks also we can assign them to the containers while creatng the containers.
# host network is directly we are giving host ip for access the container.
# none means we are not giving anything to the container so we can not access the container from outside of host.

- to show the networks
# docker network
- to create the network
# docker network create networkname

- to list the docker networks
# docker network ls
- to remove the network
# docker network rm networkname
- to inspect the network
# docker network inspect networkname


volumes # important

- so we run the container and inside the container we created some files atleast the container will store logs right. so once we remove the container again we run the container the data what we placed and along with the log file also not available. 
- this type of data we wll call thin writable data.

- so for overcome of this we have to create one folder and bind mount to the contaier

# docker run --mount source=foldername,destination=destinationpath ./app <imagename>
# docker run --volumes-from con1 /app
# docker run -v sourcepeth or (volumename):destination path

we will create the volumes in localhost or any outside of dockerhost like s3 bickets, storage account and bind to the container.
-- in this case the container deleted and we can run the container incluing bind the volume the caontainer wont lost the data. 

- to show the volumes
# docker volume
- to show the all the volumes
# docker volume ls
- to create the volumes
# docker volume create volumename
- to remove the volumes
# docker volume rm volumename
- to inspect the volume
# docker volume inspect volumename


# if the application very huge we divided into the mocroservises at this point of you we have to use docker compose. because we divided into smaller chungs each chang is nothing but one container these containers are depends eachother.

so we can use docker-compose and we write the single yaml file inside that we mention everything for run the containers.

the yaml file consists of 

version: 3.8
services:
 container1:
  restarts:always
  build:
   context: .service
   dockerfile:dockerfile
  ports:
   -5000:5000
  volumes:
   - volumename:./app
  network:
   - networkname 
 container2:
  restarts:always
  image:imagename
  ports:
   -8080:80
  volumes:
   - volumename
  dependson:container1
 container3:

volumes:
 volume1
 driver:bridge
network:
 networkname
 driver:bridge



 # finally still now we discussed about how to write the docker file, docker compose file.
 but without writing the dockerfile and docker-compose files we can create by using docker init.
 # docker init its automatically creates the dockerfile, docker-compose file.



 # here in this senario nginx acts as a load balancer. when you will hit on this load balancer the request send the server1 second request will send to the server2 again it will send to the server1 then server2.
 # in this senario we used robin-wood machanisam in this mechanisam the requests will send the containers one by one and again it will start from first to last.

 # you just clone this repository and exicute docker compose up. you can able to access the containers throught your browser.
 # docker compose up -d   

 the 1st requst will send to the server1
 
 the 2nd requst will send to the server2
 
 the 3rd requst will send to the server1
 
 the 4th requst will send to the server2
 
 the 5th requst will send to the server1
 
 the 6th requst will send to the server2
 
 the 7th requst will send to the server1
 
 the 8th requst will send to the server2


 # pls make sure docker will install in your server before exicuing the command 

 # once done  # docker compose down to delete the containers 
 # docker compose down

