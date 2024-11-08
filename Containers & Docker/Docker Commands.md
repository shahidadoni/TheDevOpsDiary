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
    - /bin/bash is used to start a bash session in terminal you can put other shell session that is supported by the image
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


