## Container Image
- Packaged Application with all dependencies and configurations
- Portable artifact that can be shared/moved around easily.
- They live in a container repository like DockerHub.
- Containerization solved dependency version conflicts and misunderstandings between development and operations team.


## Container
- Container consists layers of container images/docker images.
- Container can be run to start an application and they have their own isolated environment
- Mostly container would have a base image like linux alpine and an application image layer on top of it related to the usecase.

## Container vs Container Image
- Container is a running environment for container image
- Container have their virtual filesystem and ports that can be binded with the host port.
- Container Image on the other hand is an application package which can be ran on a Container

## OS as a Image and its layers
- OS have 2 layers OS kernel layer and application layer which sits on top on OS kernel layer
- The OS kernel layer is heart of a machine and it translates commands from applicaion layer to the hardware in machine code
- The OS kernel layer sits on top of hardware
- Example for this is we have a linux kernel but many distribution like ubuntu, centos, amazon linux, etc which are basically applications layer

## Containerization (Docker) vs Virtualization(VM)
- Docker containerizes the applications layer 
- Whereas virtualization is package/container of OS layer and Application layer
- This means docker uses the host os layer and vm boots up its own.
- Containers are lightweight in size MBs to GBs when compared to Vms and they spinup, start, stop and run much faster.
- On the other you can run a VM of any OS on any host OS. To do this in docker is a bit complex and you would need to make use of docker toolbox which abstract your host os to run a docker image on any os.

## Docker Components and Architecture
1. Docker Engine
    - Server: pulling images ad managing images and containers
        1. Container Runtime: Pulling images, starting and stopping containers. Managing container lifecycle.
        2. Volumes: To persist data
        3. Network: for communication between containers and configurations
        4. Building images: for building own images
    
    - API: Interacting with docker server
    - CLI: CLI client to execute docker commands

## Docker Network
- Containers inside a host can communicate with each with just names as they reside in the same docker network. Like mongoDb communicating mongoDb-express-ui. A backend app running on node outside the container can connect to the containers via host ip and port

## Docker Compose File

```yaml
version: '1'
services:
    mongodb:                 #container_name
        image: mongo         #image_name
        ports:
            - 27017:27017    #portbinding host:container
        enviroment:
            - MONGO_INITDB_ROOT_USERNAME=shahid
            - MONGO_INITDB_ROOT_PASSWORD=123456
        volumes:
            - mongo-data:/data/db    #for mysql it could be /var/lib/mysql/data
    
    mongo-express:                 #container_name
        image: mongo-express       #image_name
        ports:
            - 8080:8080            #portbinding host:container
        enviroment:
            - ME_CONFIG_MONGODB_ADMINUSERNAME=shahid
            - ME_CONFIG_MONGODB_ADMINPASSWORD=123456
            - ME_CONFIG_MONGODB_SERVER=mongodb
volumes:
    mongo-data:
        driver: local
```

- As you can see we haven't created a network in docker compose file. Docker compose takes care of creating a common network for the containers for them to communicate with each other

## Docker File
- Blueprint for creating and building dockers images
```dockerfile
From node          #image_name

ENV MONGO_INITDB_ROOT_USERNAME=shahid \ MONGO_INITDB_ROOT_PASSWORD=123456

RUN mkdir -p /home/app     #execute any linux command inside the container

COPY . /home/app       #Copies file from your host onto the container

CMD ["node","/home/app/server.js"]     #Run command after build process mostly to start the application

```

- RUN is used to execute commands during the build process of a Docker image, while CMD is used to specify the default command to run when a Docker container is started from the image.
- RUN is for image build-time actions (like installing dependencies).It is executed during the image build.
Cannot be overridden by user input.
- CMD is for setting the default behavior of the container at runtime (what the container runs when it starts).It is executed when the container starts.can be overridden at runtime by providing a command in docker run.
- The name of the file is strictly to be named as "Dockerfile".

## Image Naming in Docker Registries and pushing them
- registryDomain/imageName:tag
- In DockerHub it does using shorthand imageName:tag but in the backend the it pull from docker.io/library/mongo:4.2
- For renaming locally build image and pushing it to a repository for example aws ecr, we can use `docker tag shahid-app:1.0 1234567890.dkr.ecr.ap-south-1.amazon.com/shahid-app:latest` and then `docker push 1234567890.dkr.ecr.ap-south-1.amazon.com/shahid-app:latest`

## Docker Volumes
- For persisting data across containers. Data is gone when container is restarted or stopped/started.
- How this works is data gets replicated from containers virtual filesystem to host's file system. For example, containers /var/lib/mysql/data gets replicated onto host's /home/mount/data and vice versa when container restarts.
- There are 3 types of volumes
    1. Host volumes: `docker run -v host_directory:container_directory`. You specify the host and container volume directory
    2. Anonymous volumes: In this case `docker run -v container_directory`, you dont specify the host directory for volume and docker decides on its own which basically resides under /var/lib/docker/volumes/random-hash/_data
    3. Named volumes: Same as anonymous where docker decides where to store data but with name and not random-hash. `docker run -v name:container_directory`
- The recommended method it to use named volumes in production.
- When using anonymous or named volumes, you can find docker volumes locations at C:\ProgramData\docker\volumes for windows and /var/lib/docker/volumes for linux and mac os.

## Making Docker available inside container by copying it from host
 ```shell
 docker run -p 8080:8080 -p 50000:50000 -d \
 > -v jenkins_home:/var/jenkins_home \
 > -v /var/run/docker.sock:/var/run/docker.sock \ #for docker
 > -v $(which docker):/usr/bin/docker jenkins/jenkins:lts #for docker
 ```
- Followed by this there would be error of permission. In our case the error would be related to no permissions to service user (jenkins user). You need to enter as root user inside container and run below commands
    1. `docker exec -u 0 -it <container_id> bash`
    2. `chmod 666 /var/run/docker.sock`
- You can exit and then enter into the container as normal user without using u flag

## Creating Insecure Registries Docker Configuration on Host
```json
{
    "insecure-registries":["167.99.248.163:8083"]
}

```
- save this in `vim /etc/docker/daemon.json` and `run systemctl restart docker`
- when you restart docker all the container are killed/stopped and also you need to set the same permission for docker.sock file inside container `chmod 666 /var/run/docker.sock` by entering as root user `docker exec -u 0 -it <container_id> bash`



