portbinding:
1. When we run a container which has nginx running  in that. Which runs on port 80.
2. When we try to access from local host like localhost:80. It will show error. 
3. The container is isolated from our host. So, WHen we run localhost:80 it's tries to display the content on port 80 in localhost. Since, container is isolated, it doesn't reflect in our host.
4. For that we need to use portbdinding concept, docker run -d -p 80:80 nginx:v2, then if our port80 on our localhost is free it will run nginx on that port.
5. Now, if we give localhost:80 we can see the nginx home page