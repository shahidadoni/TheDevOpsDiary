1. docker ps
    - shows/lists all the containers that are running.
    - a flag: shows/list all the containers that are running and stopped

2. docker run <image_name:version>
    - checks if the image is present in the local if not downloaded it download from the dockerhub and runs the images (start a new container)
    - A particular image also consists layer of images which are basically its dependencies or files
    - this command runs docker pull and then starts the container
    - docker pull downloads only layer of images which don't exist in your local
    - if version is not provided it download the latest version which is basically image_name:latest
    - d flag: run the container is detached mode and returns the containerId. Detached mode is basically running the container like a service in background and you can have access to the same shell session for running commands
    - p<host_port:container_port> flag: for binding your container port with the host port. Helps in case you have 2 different version of same application container running on same host with same ports. Like 2 redis container with different version. You can bind the it to 2 different host_ports like 5000 and 5001.
    - name flag: to give name for the container if left blank random name will be allotted by docker
    - network/net flag: specify a docket network where you want to run the docker network
    - e flag: for specifying environment variable. Example: -e MONGO_INITDB_ROOT_USERNAME=shahid \ -e MONGO_INITDB_ROOT_PASSWORD=123456. This is are availble in the documentation of the particular image in dockerhub
    - v flag: TO give location for persistent volume. Ex: docker run -v host_directory:container_directory or docker run -v container_directory or docker run -v name:container_directory

3. docker images 
    - shows/lists all the images present in your local
    
4. docker start <container_id>
    - Starts a stopped container

5. docker stop <container_id>
    - Stops a running container

6. docker logs <container_id>or<container_name>
    - Checking logs of the container for debugging purposes
    - f flag: to stream the logs
    - | tail: to get last 10 lines of the logs

7. docker exec -it <container_id>or<container_name> /bin/bash
    - it stands for interactive terminal
    - /bin/bash is used to start a bash session in terminal you can put other shell session that is supported by the image like /bin/sh.
    - this command is used to navigate the file system virtually using the terminal for debugging or checking file configuration or printing out environment variables
    - use exit command to quit from interactive terminal
    
8. docker network ls
    - lists all network present locally

9. docker network create <network_name>
    - Creates a new network where you can run containers

10. docker compose -f <file_name> up
    - Creates and runs every container mentioned in the docker-compose file with all the mentioned configurations

11. docker compose -f <file_name> down
    - Stops and removes every container mentioned in the docker-compose file with all the mentioned configurations
    - Also removes the default network created by the docker compose command/file

12. docker build -t <image_name:version> <Dockerfile_path>
    - The t flag is the tag you want to give to your image which basically image name and version.
    - Dockerfile path is the directory where your dockerfile resides
    - Example: `docker build -t shahid-app:1.0 .`

13. docker rm <container_id>
    - Deletes the container in your local

14. docker rmi <image_id>
    - Deletes the docker image in your local
    - First you need to check if this image is used by any container that is stopped or running then delete the respective containers.

15. docker login
    - for authenticating into private repository such as aws ecr
    - all authentication config can be found in your home directory path i.e. ~/.docker/config.json

16. docker tag <image_name:version> <new_image_name:version>
    - For renaming docker image present locally
    - Ex: docker tag shahid-app:1.0 1234567890.dkr.ecr.ap-south-1.amazon.com/shahid-app:latest

17. docker push <image_name:version>
    - Pushing docker images to repositories like dockerhub, aws ecr, etc.
    - Ex: docker push 1234567890.dkr.ecr.ap-south-1.amazon.com/shahid-app:latest
    - In the backend this command pushes the image layer by layer as in case of pulling

18. docker volume create --name <volume_name>
    - Creates a new named docker volume for persisting data

19. docker volume ls 
    - lists/shows all docker volume present in the local
    - you can also use `docker inspect <volume_name>` to get properties of volume including creation date, host mountpoint/volume location and driver.



