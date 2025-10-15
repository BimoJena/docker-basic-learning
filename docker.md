-------------------DOCKER COMMANDS--------------------



==> pull the image ***(docker pull <image-name>)***



==> check which image exist ***(docker images)***



==> build container from image ***(docker run <image-name>)***

	***==> docker run command se hum hamesha kisi bhi image se ek new container create krte hai***



==> cmd se apne image k container k terminal me jaane k liye ***(docker run -it <image-name>)*** 

&nbsp;	==> uske personal terminal se nikalne k liye ***(exit)*** isse wo container stop ho jaaega



==> to check how many containers are there present ***(docker ps -a)*** -a is for all containers



==> to check how many containers are running right now ***(docker ps)***



==> to start an existing container ***(docker start <container-id> or <container-name>)***



==> to stop an existing container ***(docker stop <container-id> or <container-name>)***



==> to delete the container (docker rm <container-id> or <container-name>)



==> to delete the images (docker rmi <image-id> or <image-name>)

&nbsp;	==> to delete images, we must have to delete the container first which were created by that image

&nbsp;	==> delete multiple images or containers ***(docker rm <continer1> <container2> ......)***



-----------------------VERSIONS \[DOCKER COMMANDS]---------------



==> to pull any specific version of image ***(docker pull <image-name>:version)***



==> to run a container in background with detach mode \[-d] ***(docker run -d <image-name>)***



-----------------------PORT BINDING--------------------------





==> connect hostport with container port ***(docker run -phostPort:containerPort <image-name>)***



==> two containers cannot bind with the same hostPort at a time





----------------------TROUBLESHOOT COMMANDS-----------------------



==> to check the logs of any container to find the error ***(docker logs <container-name> or <container-ID>)***



==> to execute the container in bash terminal ***(docker exec -it <container-ID> /bin/bash)***



==> to execute the container in shell terminal ***(docker exec -it <container-ID> /bin/sh)***







-----------------------NOTE-----------------------------



there are different layers present in images but only the container layer is mutable in nature no other layers are mutable





---------------------DIFF between DOCKER vs VIRTUAL MACHINE ---------------------------------





==> harr machine ki ye hierarchy order rehti hai



application layer

&nbsp;	|

&nbsp;	|

&nbsp; host OS layer

&nbsp;	|

&nbsp;	|

&nbsp;   hardware



==> VIRTUAL MACHINE \[host os layer] and \[application layer] dono ko virtualize krta hai, khudka os use krti hai--------------------slow and heavy size

==> DOCKER sirf \[host os layer] pe operate hota hai or \[application layer] ko virtualize krta hai----------lightweight and faster





==> VM advantage :- compatible to all os qki wo host os layer ko bhi virtualize kr deti hai

==> DOCKER disadv :-  ye chiz DOCKER nahi kr paati qki wo dependend hai host OS layer pe \[linux] based hai



docker Linux based system k liye hai to wo hum another OS pe kaise run krte hai?

==> with the help of \[DOCKER DESKTOP] it contains lightweight VM based on Linux which is compatable for all other OS





--------------------------DOCKER NETWORK-----------------



==> to check how many network are present there ***(docker network ls)***



==> to create a new docker network ***(docker network create <new-network-name>)***



==> to delete network ***(docker network rm <network-name>)***





-------------------------------MONGODB CONNECTION----------------------------



==> mongo-express:-  it gives the user interface which show details about the mongo db database 

&nbsp;	--> isko run krne ka command 

docker run -d -p8081:8081 --name mongo-express --network mongo-network -e ME\_CONFIG\_MONGODB\_ADMINUSERNAME=admin -e ME\_CONFIG\_MONGODB\_ADMINPASSWORD=qwerty -e ME\_CONFIG\_MONGODB\_URL="mongodb://admin:qwerty@mongo:27017" mongo-express





==> mongo:- it is the actual database 

&nbsp;	--> isko run krne ka command

docker run -d -p27017:27017 --name mongo --network mongo-network -e MONGO\_INITDB\_ROOT\_USERNAME=admin -e MONGO\_INITDB\_ROOT\_PASSWORD=qwerty mongo







--------------------------EXAMPLE----------------



* humne ek project banaya jisme koi database nahi rkha hai local system pe sirf frontend and backend banaya hua hai
* ab hume apne container pe ek mongo db setup krna hai 
* yahi chiz hum kr rhe hai







-----------------------------DOCKER COMPOSE---------------------------------------



* docker compose is a tool for defining and running multi-container applications
* dekho simple hai------------
* jab hum multiple container banate the jisme bht saare extra attributes hote hai like environment variable, name, network, etc jaisa \[mongo] and \[mongo-express] k containers banana k time hua tha itna complex naa ho isliye better hai ek .yaml (yet another markup language) file bana denge usme saari info hogi or ab container ko terminal se run krne k bajaye file se run karenge



* benefits \[structured way] and \[edit krna easy hoga]
* .yaml se container ko run krne k liye hum compose ko use karenge



* jab bhi .yaml file create krte hai hume koi network mention krne ki need nahi hoti qki bydefault phle ek default network create hoga or jo jo images uss .yaml file me hai wo sb usi same network k ander create hogi



* syntax



version: "3.8"



services: 

&nbsp; mongo: 

&nbsp;   image: mongo

&nbsp;   ports: 

&nbsp;   - 27017:27017

&nbsp;   environment: 

&nbsp;     MONGO\_INITDB\_ROOT\_USERNAME: admin

&nbsp;     MONGO\_INITDB\_ROOT\_PASSWORD: qwerty



* run command ***(docker compose -f filename.yaml up -d)***



* delete all the containers ***(docker compose -f filename.yaml down)***





-----------------------------CREATING OWN APP INTO DOCKER-------------------------



==> docker file instructions





* FROM <image> - this specifies the base image that the build will extend.
* WORKDIR <path> - this instruction specifies the "working directory" or the path in the image where files will be copied and commands will be executed.
* COPY <host-path> <image-path> - this instruction tells the builder to copy files from the host and put them into the container image.
* RUN <command> - this instruction tells the builder to run the specified command.
* ENV <name> <value> - this instruction sets an environment variable that a running container will use.
* EXPOSE <port-number> - this instruction sets configuration on the image that indicates a port the image would like to expose.
* USER <user-or-uid> - this instruction sets the default user for all subsequent instructions.
* CMD \["<command>", "<arg1>"] - this instruction sets the default command a container using this image will run.



\# \[run] command can be multiple but \[cmd] command is only one.



SNNIPET



FROM python:3.13

WORKDIR /usr/local/app



\# Install the application dependencies

COPY requirements.txt ./

RUN pip install --no-cache-dir -r requirements.txt



\# Copy in the source code

COPY src ./src

EXPOSE 8080



\# Setup an app user so the container doesn't run as the root user

RUN useradd app

USER app



CMD \["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8080"]





* to build the docker image from the application ***(docker build -t <file-name>:<version>)***



* 

















