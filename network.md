## Network in Docker: ##
 By default When Docker is installed, it automatically creates three default networks:
 + Default bridge network
 + Host network
 + None network
+ We can see the network list in the docker using **_`docker network ls`_**
```
docker network ls
  NETWORK ID     NAME      DRIVER    SCOPE
55954fb00e3c   bridge    bridge    local
478067e86f8f   host      host      local
ff7f2f1b7695   none      null      local
```
### _Default Bridge Network:_ ###
   + This network interface is called “docker0”.
   + This creates a private network and are isolated from the host system. To allow the access to the host we use port mapping, where we map available port of host to the port of the container using -p- flag at the time of creating a container. 
   + when we create any container by default this network is attached to the container.
   + This network assigns an ip address to the containers.
   + Containers on this network can communicate with each other internally with the ip address.
   + But, this default bridge network does not provide automatic DNS resolution between containers.
      + Let's say we have two containers mongodb and backend and they are on the same network. We want to connect to the mongodb container from the backend container.
      + When we try to connect using ip address of the mongodb, it connects without any issue.
      + But, when you try to connect with the container name **_mongodb_**, it cannot connect and throws an error.
      + This is because it couldn't resolve the DNS name of mongodb into ip address. So, connection fails.
      + To resolve this issue _we need to create our own network_ and assign it to containers while creating.
### _Host Network:_ ##
   + As name suggests it used the Docker host network.
   + Since, it's using host network. There is no isolation between the container and the Docker host.
   + Running a web server container on port 8080 will make the server immediately accessible on the host’s port 8080 without any additional port mapping. Unlike the bridge network where we need to map the host port to container port
   + The drawback is multiple containers cannot simultaneously use the same port on the host. As that port is already occupied by another service.
### _None Network:_ ##
   + The none network disconnects the container from any networking, ensuring complete isolation from external networks and other containers.
   + We use this when we want our container cannot be accessed by any other network.
   + We rarely use this network, Used for testing, security isolation, or compute-only jobs that require no network connectivity at all.
## _User defined network:_ ##
   + We can create our own network. Here, we are creating a network named project.
       ```
     docker network create project
       ```
   + To use this network, we need to assign it to the container when creating.
      ```
     docker run -d --name=frontend --network=project frontend:v1
      ```
   +  This will support DNS resolution by default.
   +  Containers on the same network can communicate using their container names instead of IP addresses. As container IP addresses can change when the system reboots.
   +  We can check this with the command below and at the bottom of the output it's mentioned the DNS Name 
       ```
      docker inspect <<container_name/id>
       ```
```
    "IPPrefixLen": 16,
  "IPv6Gateway": "",
  "GlobalIPv6Address": "",
  "GlobalIPv6PrefixLen": 0,
  "DNSNames": [
      "mongodb",
      "4b86a1654331"
  ]
```

       
