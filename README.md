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
+ By default all the images or anything which we are creating will be stored in `/var/lib/docker`
  
```
docker run my-image
```
### Container Creation 
+ Containers created from the image use this snapshot and do not re-run the Dockerfile’s build instructions (e.g., RUN, COPY).
+ When you create a container with docker run my-image, Docker uses the image as a template to create a running instance (the container).
+ The container starts with the exact filesystem and configuration stored in the image. **_It does not re-execute the Dockerfile instructions (e.g., it doesn’t re-run apt-get install or re-copy files)_**.
+ **_runtime commands: RUN, ENTRYPOINT_**
+ The container uses the pre-installed software and files from the image. No installation steps from the Dockerfile are repeated unless explicitly triggered by a command (e.g., CMD or ENTRYPOINT) that runs when the container starts.
+ Containers are meant to run a specific task or process. It will live as long as a process/task is alive. Once it's done it exits. For example, if you hosted a webserver like nginx, when it's stopped then container aloso stops.
  + **Service Running in Foreground**: If the service (e.g., Nginx) is running as the main process (PID 1) in the container, stopping the service will typically stop the container itself, as Docker containers exit when their main process stops.
  + **Service Running in Background**: If the service is running in the background (e.g., started with a script or daemon on), you can stop it without stopping the container.
+ In this stage it only does actions which are instructed with **_CMD_** or **_ENTRYPOINT_** commands mentioned in the docker file. These comamnds runs evrytime when container starts. So, installations are mentioned in Dockerfile’s RUN command during the build.
+ **_Each docker run creates a new container even if the image is same_**

## commands :
+ To pull the image from the docker hub. eg: _docker pull nginx:latest_
  ```
  docker pull <image_name>:<tag/version>
  ```
+ To push your image to the repository, we can push to either docker Hub, Amazon ECR, or a private registry. If we are using docker hub then the format should be `docker push docker.io/username/image:tag`. For example, let's say we have created an image with name **docker build -t test:v1**
  + We don't need to provide _"docker.io"_. By default docker considers it.
  + We need to have username to our repository. test is the repository. So, it should be named as username/test:v1. When we       tagged with dd070/test:v1. then when you give docker images comamnd. You'll see two repositories with the same image id.      We have two tags pointing to the same Image ID. There’s no duplication of the image’s content; it’s just two labels for       the same image.
    
     ```
     docker tag test:v1 dd070/test:v1
    ```
  +  Now, we can push to docker hub using
     ```
     docker push dd070/test:v1
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
+  The equalent of all the above steps(pull, create & start) for running a container is, to run in the background use -d (detach mode). It checks if image is present lcoally if not it will download from the docker hub and place it in the local.
   ```
   docker run -d <image_name:tag>
   ```
+ Docker containers are isolated from the host system, including their network stack. When a container runs a service like Nginx on port 80 inside the container, this port is not automatically accessible from the host or external network. If you try to access localhost:80 on the host, you'll get an error (e.g., "connection refused") because the host's port 80 is not linked to the container's port 80.
+ We use **_port binding_** concept to tackle this issue, Port binding is the process of mapping a port on the host to a port inside the container. This is done using the -p (or --publish) flag in the docker run command.
+ After running the above command, if port 8080 on the host is free, Docker binds the host's port 8080 to the container's port 80.
+ Now, when you access localhost:8080 (or 127.0.0.1:8080) from the host, the request is forwarded to port 80 inside the container, where Nginx is listening, and you'll see the Nginx home page.
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
## _RUN vs CMD_ ##
  ## RUN: ##
  + RUN command will execute when image is created
  + we use this to run any commands in the container like installing any packages, commands, etc.
  + We can have any number of RUN commands in the docker file.
  ## CMD: ##
  + CMD comamnd will execute during container creation
  + The arguments which are in CMD commands will be executed as soon as the container starts by default
  + There can only be one CMD instruction in a Dockerfile. If you list more than one CMD, only the last one takes effect.
## _ENTRYPOINT:_ ##
+ It's a command which executes during runtime of the container.
+ It's similar to the CMD command. But difference is CMD comamnd can be replaced/overriden with CLI but ENTRYPOINT command cannot be replaced/overridden.
+ Instead CLI command gets appended to the ENTRYPOINT command.
+ Let's say you gave **_CMD ["sleep", "10"]:_**
   + When user gives **docker run -d test1:v1**. Then it sleeps for 10secs and exits the container.
   + When he gives **docker run -d test1:v1 sleep 5**. Then it sleeps for 5secs and exits the container.
   + So, CMD is an replacable container and if he gives some random command in CLI then container throws an error.
+ Let's say you gave as **_ENTRYPOINT ["sleep", "10"]:_** 
    + When user  gives **docker run -d test1:v1**. Then it sleeps for 10secs and exits the container.
    + When he gives **docker run -d test1:v1 sleep 5**. Then container appends it like sleep 10 sleep 5 & throws an error.
    + Since, it's doesn't replace the command with CLI command. So, it's more robust & best for security.
 + **Combination of CMD & ENTRYPOINT**:
   + We can use combination of both like for a flexible execution.
       ```
       ENTRYPOINT ["sleep"]
       CMD ["5"]
       ```
   + When user gives docker run -d test1:v1. Then it sleeps for 5 secs and exits the container.
   + When user gives docker run -d test1:v1 20. Then it sleeps for 20 secs and exits the container. Since, CMD is replacable 5 is replaced with 20 and ENTRYPOINT appends it.
        
## _COPY:_ ##
  +  It is used to copy any files & directories from local path to the image
     ```
     COPY <source> <destination>
     ```
## _ADD:_ ##
  + Add also does copy of files/diectories from source to destination simiarly like COPY. But it has two more additional          features,
     1. It can download any files from the internet. We need to give read permission for the index.html
       ```
          ADD https://raw.githubusercontent.com/daws-84s/roboshop-docker/refs/heads/main/cart/Dockerfile /usr/share/nginx/html/index.html
       ```
     2. It can directly `untar` the files into the container
        ```
         ADD file.tar /tmp/folder
         ```
 ## _EXPOSE:_ ##
  + It is like documentation between the person who builds the image and the person who runs the container, about which           ports are intended to be published. It doesn't do anything. Just for documentation purpose.
       ```
      EXPOSE 80
       ```
 ## _USER:_ ##
  + By default when we create an container it will do all the actions as a root user. Which is risky
  + You can check by login into the container you can see as (#)
  + So, we need to use normal user. After container is created. Now, all operations are runned as test user
    ```
    USER test
    ``` 
## _WORKDIR:_ ##
  + We cannot `cd` in container. So, we need to use **WORKDIR /tmp**. It will go to temp directory.
     ```
    WORKDIR /tmp
     ```
## _ENV:_ ##
+ The ENV instruction sets the environment variable <key> to the value <value>
    ```
    ENV COURSE="docker"
  ```
 + The ENC variable persists in container also
## _ARG:_ ##
 + It's similar to ENV but it's scope is only during building of an image. Cannot be accessed inside container.
 + Usually the first command in the docker file is FROM command but ARG can be placed in line1. But after from instruction we    cannot use that variable.
    ```
   ARG user1=someuser
    ```
  + It can also pass during build like
    ```
    docker build -t test2:v1 --build-arg user1=rocky .
    ```
  + How can you access the ARG variable insider container also?
     + By assigning the ARG value to the ENV value with the same key. Like
     ```
        ARG username = "rocky"
        ENV username = ${username}
       ```
   + ## _ONBUILD:_ ##
     + When we write a docker file to create an image we use ONBUILD command.
     + It's applicable when any other user uses the image built from this dockerfile.
     + The ONBUILD command is not executed when this docker file is build.
     + For example, if user A writes this docker file and created an image, then the onbuild command is not executed for this        userA when image was built. When person B used the image file created by person A then ONBUILD get activates.
        ```
       ONBUILD COPY index.html /usr/share/nginx/html/index.html
        ```    
    





