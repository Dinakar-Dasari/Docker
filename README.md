```
docker build image
```
### working in background: 
+ All installation and configuration steps in the Dockerfile are executed only during the build process. The image captures the result of these steps, not the instructions themselves.
+ Docker processes the Dockerfile line by line, executing each instruction (e.g., FROM, RUN, COPY, ADD, ENV, etc.)
+ Each instruction creates a new layer in the image. These layers are combined to form the final image.
+ The final image is a read-only snapshot of the filesystem, including all installed software, dependencies, and configurations specified in the Dockerfile
+ **_The image is a snapshot of the filesystem and configuration after executing the Dockerfile_**
+ **_Layer Caching_**: Docker caches image layers to speed up builds. If you change a line in the Dockerfile, only that line and subsequent ones are re-executed during a rebuild.
  
```
docker run my-image
```
### Container Creation 
+ Containers created from the image use this snapshot and do not re-run the Dockerfile’s build instructions (e.g., RUN, COPY).
+ When you create a container with docker run my-image, Docker uses the image as a template to create a running instance (the container).
+ The container starts with the exact filesystem and configuration stored in the image. **_It does not re-execute the Dockerfile instructions (e.g., it doesn’t re-run apt-get install or re-copy files)_**.
+ **_runtime commands: RUN, ENTRYPOINT_**
+ The container uses the pre-installed software and files from the image. No installation steps from the Dockerfile are repeated unless explicitly triggered by a command (e.g., CMD or ENTRYPOINT) that runs when the container starts.
+ In this stage it only does actions which are instructed with **_CMD_** or **_ENTRYPOINT_** commands mentioned in the docker file. These comamnds runs evrytime when container starts. So, installations are mentioed in Dockerfile’s RUN command during the build.
+ **_Each docker run creates a new container even if the image is same_**

## commands :
+ To pull the image from the docker hub  eg: _docker pull nginx:latest_
  ```
  docker pull <image_name>:<tag/version>
  ```
+  To see all the images
     ```
   docker images
     ```
+ To remove an image
  ```
  docker rmi <image_name/ID>
  ```
+ To create a container. It only creates a container. Docker assigns random name to have your custom name give, ***docker create --name "dd" nginx:1.29.0***
   ```
  docker create nginx:latest
   ```
+ To start the created container.
   ```
  docker start <container-id/name>
   ```
+  The equalent of all the above steps(pull, create & start) for running a container is, to run in the background use -d (detach mode)
   ```
   docker run -d <image_name:tag>
   ```
+ By default container ports are not exposed to public, for eg: an app server is hosted in a container. usually public cannot    see it. For that purpose, we Maps host port to container port. Host port should be free, means no other service should run     on it. FOr example if you mapped on 8080, when you type <pc_ip>:8080 then can see the content present in the container
  ```
  docker run -d -p 8080:80 <container_id>
  ```
  
+ to access or get into the running container use bash/shell based on the os
  ```
  docker exec -it <container_id> bash/shell
  ```
+ To get the detailed information about the images, container, volume & other resources,  we use docker inspect command. It      provided low level details and This command is particularly useful for troubleshooting and understanding the configuration     of Docker environments
    ```
    docker inspect <id/name>
    ```
+ To list all the ***_running container_***
   ```
  docker ps
   ```
+ To list all the ***_containers_***
  ```
  docker ps -a
  ```
+ To remove a container and -f for force
    ```
    docker rm <container_name/ID>    
  ```
+ To remove all containers and -f for force
   ```
    docker rm `docker ps -aq'
    ```
+ To remove only stopped container
   ```
  docker container prune
   ```
+ To see the logs of a container use, To view logs in real-time, you can use the --follow or -f flag
   ```
   docker logs <container_name/ID>
   ```
+ To build an image from our docker file
  ```
  docker build -t <docker_file> <directory_of_docker_file>
  ```
+ ## _RUN vs CMD_ ##
    ## RUN: ##
  + RUN command will execute when image is created
  + we use this to run any commands in the container like installing any packages, commands, etc.
  + We can have any number of RUN commands in the docker file.
  ## CMD: ##
  + CMD comamnd will execute during container creation
  + The arguments which are in CMD commands will be executed as soon as the container starts by default
  + There can only be one CMD instruction in a Dockerfile. If you list more than one CMD, only the last one takes effect. 
  





