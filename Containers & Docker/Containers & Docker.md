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
    
    mongo-express:                 #container_name
        image: mongo-express       #image_name
        ports:
            - 8080:8080            #portbinding host:container
        enviroment:
            - ME_CONFIG_MONGODB_ADMINUSERNAME=shahid
            - ME_CONFIG_MONGODB_ADMINPASSWORD=123456
            - ME_CONFIG_MONGODB_SERVER=mongodb
```

- As you can see we haven't created a network in docker compose file. Docker compose takes care of creating a common network for the containers for them to communicate with each other
