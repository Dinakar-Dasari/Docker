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









