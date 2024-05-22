# DOCKER DOCUMENTATION
Docker simplifies application creation, deployment, and running through containers, providing a convenient utility for packaging and running applications in a semi-isolated environment.


### References.
- https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04
- https://www.geeksforgeeks.org/docker-cheat-sheet/


----

## Version History.

- Version: 1.0, Author: Sandeep Patel, Last Updated: May 20th, 2024, Created: April 17th, 2024.

-------


# Table of Contents:
- [1. INTRODUCTION TO DOCKER:](#introduction-to-docker)
- [Installation Commands on Ubuntu](#installation-on-ubuntu)
- [Basic Terminology]()
  - [Dockerfile](#dockerfile)
  - [Image](#image)
    - [Image Management Commands]()
    - [CUSTOM IMAGES:]()
    - [Image Transfer Commands]()
  - [Container](#container)
- [Docker Login And Pushing Docker Images to a Docker Repository](#docker-login-and-pushing-docker-images-to-a-docker-repository)
- [Docker Exposing Ports Commands](#docker-exposing-ports-commands)
- [DOCKER COMPOSE:](#docker-compose)
- [Key Differences Between Docker File and Docker-Compose File](#Key-Differences-Between-Docker-File-and-Docker-Compose-File)
- [Methods for Creating Volumes in Docker](#Methods-for-Creating-Volumes-in-Docker)
- [Docker Swarm Commands](#docker-swarm)
- [Scaling Containers](#scaling-containers)
- [Methods For Providing ENV to docker container](#Methods-For-Providing-ENV-to-docker-container)
- [Viewing Docker Logs:](#viewing-docker-logs)
- [Kafka And ZooKeeper](#kafka-and-zooKeeper)



----


## INTRODUCTION TO DOCKER:
Docker is a platform for developing, shipping, and running applications inside containers. Containers allow developers to package an application and its dependencies into a standardized unit for software development, making it easy to deploy applications across different environments.
(sub contents must be started with triple # )

### Installation on Ubuntu
First, update your existing list of packages:
```
sudo apt update
```
Next, install a few prerequisite packages which let apt use packages over HTTPS:
```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```
Then add the GPG key for the official Docker repository to your system:
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
Add the Docker repository to APT sources:
```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
```
This will also update our package database with the Docker packages from the newly added repo.

Make sure you are about to install from the Docker repo instead of the default Ubuntu repo:
```
apt-cache policy docker-ce
```
You’ll see output like this, although the version number for Docker may be different:
<img width="517" alt="Screenshot 2024-04-29 112312" src="https://github.com/sandeeppatel2001/Docker_doc/assets/95873801/16fae37a-db6a-4fdc-956a-0de0f66a0710">
- Notice that docker-ce is not installed, but the candidate for installation is from the Docker repository for Ubuntu 20.04 (focal).
- Finally, install Docker:
```
sudo apt install docker-ce
```
Docker should now be installed, the daemon started, and the process enabled to start on boot. Check that it’s running:
```
sudo systemctl status docker
```
The output should be similar to the following, showing that the service is active and running:

<img width="503" alt="Screenshot 2024-04-29 112530" src="https://github.com/sandeeppatel2001/Docker_doc/assets/95873801/30f1a73c-bd15-4db9-a529-a635e13f0ba8">

## Basic Terminology
### Dockerfile: 
A text document that contains instructions for building a Docker image. Dockerfiles define the environment inside a container, including the base image, dependencies, and runtime configuration.
### Image
A read-only template used to create containers. Images are built from Dockerfiles and can be shared and reused across different environments.
- To build a Docker image using Dockerfile, navigate to the directory containing the Dockerfile and run:
```
docker build -t my-node-app .
```
- This command builds a Docker image named my-node-app based on the instructions in the Dockerfile.
- Listing Images
```
  docker image ls
```
- From a Remote GIT Repository
```
docker build github.com/creack/docker-firefox
```
- Building from a Remote Dockerfile URI
```
curl example.com/remote/Dockerfile | docker build -f – .
```
- Removing an Image
```
docker image rm nginx
```
- Showing the History of an Image
```
docker image history
```
- Pushing an Image
```
docker image push eon01/nginx
```
### Container:
A runnable instance of an image. Containers run applications and contain everything needed to run the application, including the code, runtime, libraries, and dependencies.

Once you have Docker images, you can create and manage containers based on these images.
- Starting, Stopping, and Restarting Containers:
```
docker run <image_name>        # Start a container from an image
docker stop <container_id>     # Stop a running container
docker start <container_id>    # Start a stopped container
docker restart <container_id>  # Restart a running container
```
- Another way (let image have to run = nginx)

```
docker container start nginx    #Starting Containers
docker container stop nginx    #Stopping Containers 
docker container restart nginx  #Restarting Containers
docker container pause nginx     #Pausing Containers
docker container unpause nginx     #Unpausing Containers
docker container wait nginx       #Blocking a Container
docker container kill nginx      #Sending SIGKILL Containers
docker ps                    #Check the Containers
docker container ls          #To see all running containers
docker logs infinite         #Container Logs
docker container logs infinite -f   #‘tail -f’ Containers’ Logs
docker container inspect infinite   #Inspecting Containers
docker container stats infinite   #Container Resource Usage
docker container diff infinite   #inspecting changes to files or directories on a container’s filesystem
```
## Docker Login And Pushing Docker Images to a Docker Repository
To push your image, first log into Docker Hub.
```
docker login -u docker-registry-username
```
You’ll be prompted to authenticate using your Docker Hub password. If you specified the correct password, authentication should succeed.
- Note: If your Docker registry username is different from the local username you used to create the image, you will have to tag your image with your registry username. For the example given in the last step, you would type:
```
docker tag sammy/ubuntu-nodejs docker-registry-username/ubuntu-nodejs
```
Then you may push your own image using:
```
docker push docker-registry-username/docker-image-name
```
To push the ubuntu-nodejs image to the sammy repository, the command would be:
```
docker push sammy/ubuntu-nodejs
```
- Logout from a Registry
```
docker logout
```
- Logout from a Registry
```
docker logout
```
---
## Docker Exposing Ports Commands
For Eposing the Port we can directly write EXPOSE [port number] in the Docker file
or during running the container we can map the port
```
docker run -p $HOST_PORT:$CONTAINER_PORT –name [container_name] -t [imageName]
```
## Docker Compose
Docker Compose allows you to define a multi-container application with all of its dependencies in a single file. This way, you can manage, configure, and start the whole stack with simple commands. It is particularly useful for microservices, where each service is in its own container.
### Compose File Structure
- To start using Docker Compose, you need to create a docker-compose.yml file that describes your services, networks, and volumes.
```
version: '3.8'  # Specifies the version of the Docker Compose file format

services:  # Defines the services that make up the application. Each service runs a container.
  web:  # The name of the service. This can be used to refer to the service in other parts of the configuration.
    image: nginx:latest  # Uses the latest version of the Nginx image from Docker Hub
    ports:  # Maps ports on the host to ports in the container
      - "80:80"  # Maps port 80 on the host to port 80 in the container, making the web server accessible via HTTP on port 80
    volumes:  # Mounts host machine directories or files into the container
      - ./html:/usr/share/nginx/html  # Mounts the local ./html directory to /usr/share/nginx/html in the container, where Nginx serves static files

  database:  # The database service
    image: postgres:latest  # Uses the latest version of the PostgreSQL image from Docker Hub
    environment:  # Sets environment variables inside the container
      POSTGRES_DB: example_db  # Creates a database named example_db
      POSTGRES_USER: example_user  # Sets the PostgreSQL user to example_user
      POSTGRES_PASSWORD: example_password  # Sets the password for the PostgreSQL user
    volumes:  # Mounts host machine directories or named volumes into the container
      - db_data:/var/lib/postgresql/data  # Mounts the named volume db_data to /var/lib/postgresql/data in the container, which is the directory where PostgreSQL stores data

volumes:  # Defines named volumes that can be shared among services or used to persist data
  db_data:  # A named volume that stores PostgreSQL data, ensuring data persistence across container restarts

```
Start your application:
```
docker-compose up
```
## Key Differences Between Docker File and Docker-Compose File
### Scope:
- Dockerfile: Defines how to build a single Docker image.
- Docker Compose file: Defines how to run and configure multiple Docker containers.
### File Type:
- Dockerfile: Typically named Dockerfile.
- Docker Compose file: Typically named docker-compose.yml.
### Commands:
- Dockerfile: Used with docker build.
- Docker Compose file: Used with docker-compose up, docker-compose down, etc.
### Configuration:
- Dockerfile: Specifies instructions for building an image.
- Docker Compose file: Specifies services, networks, volumes, and configurations for running containers.
### Usage Scenarios:
- Dockerfile: Ideal for creating a custom image for a single service.
- Docker Compose file: Ideal for running and managing applications composed of multiple services.

## Methods for Creating Volumes in Docker
Volumes in docker are the preferred way to deploy a stateful set application in the form of containers. You can manage and persist the docker data outside of the docker life cycle. Docker allows you to mount shared volumes in multiple Containers.
### Display all the existing Docker Volumes
```
sudo docker volume ls
```
output:

### To create a new Docker Volume, you can use the Volume Create Command.
Command: 
```
docker volume create sandeepvolume
```
output:

### Inspecting Docker Volumes
```
sudo docker volume inspect sandeepvolume
```
![Uploading Screenshot 2024-05-22 150440.png…]()

After creating the Volume, the next step is to mount the Volume to Docker Containers. We will create a Docker Container with the Ubuntu base Image which you will mention in Dockerfile and mount the sandeepvolume Volume to that Container using the -v flag.
Command:
```
docker run -v sandeepvolume:/app/data my_imagename
```
The above command mounts the sandeepvolume volume to a directory called app/data inside the Docker Container named my-imagename.
### Specifying Volumes in docker-compose.yml:

File Example
```
services:
  frontend:
    image: node:lts
    volumes:
      - mongovol:/data/db
backend:
    image: node:lts
    volumes:
      - db_data:/data/db
volumes:
  my_volume:
  db_data:
  mongovol:
```
- We Can Assign This volume To Different Service like here we are assigning mongovol to frontend service and db_data volume to backend volume.
### Creating Anonymous Volumes:

Command: 
```
docker run -v /app/data my_image
```
### Bind Mounts:
Command: 
```
docker run -v /host/path:/container/path my_image
```
### Using --mount Flag:

Command:
```
docker run --mount type=volume,source=my_volume,target=/app/data my_image
```
## Docker Swarm
Docker Swarm is Docker's native clustering and orchestration tool. It allows you to manage a cluster of Docker nodes as a single virtual system. Docker Swarm turns a pool of Docker hosts into a single, virtual Docker host. This enables you to scale your applications horizontally by adding more nodes to the cluster and distributing containers across these nodes.
## Scaling Containers
replicas: Specifies the number of container instances to run for the service.
```
version: '3.8'  # Specifies the version of the Docker Compose file format

services:  # Defines the services that make up the application
  web:  # The web service
    image: nginx:latest  # Uses the latest version of the Nginx image from Docker Hub
    ports:  # Maps ports on the host to ports in the container
      - "80:80"  # Maps port 80 on the host to port 80 in the container, making the web server accessible via HTTP on port 80
    deploy:  # Deployment configuration for Docker Swarm
      replicas: 3  # Specifies the number of container instances to run (3 replicas)
      update_config:  # Configuration for updating the service
        parallelism: 2  # Specifies how many containers to update at a time (2 in parallel)
        delay: 10s  # Specifies the delay between updates of a group of containers (10 seconds)
      restart_policy:  # Restart policy for the service
        condition: on-failure  # Specifies that the containers should only be restarted if they fail

  database:  # The database service
    image: postgres:latest  # Uses the latest version of the PostgreSQL image from Docker Hub
    environment:  # Sets environment variables inside the container
      POSTGRES_DB: example_db  # Creates a database named example_db
      POSTGRES_USER: example_user  # Sets the PostgreSQL user to example_user
      POSTGRES_PASSWORD: example_password  # Sets the password for the PostgreSQL user
    volumes:  # Mounts host machine directories or named volumes into the container
      - db_data:/var/lib/postgresql/data  # Mounts the named volume db_data to /var/lib/postgresql/data in the container, which is the directory where PostgreSQL stores data

volumes:  # Defines named volumes that can be shared among services or used to persist data
  db_data:  # A named volume that stores PostgreSQL data, ensuring data persistence across container restarts


```
### Deploy the stack:
```
docker stack deploy -c docker-compose.yml my_stack
```
## Methods For Providing ENV to docker container
### Using ENV in Dockerfile:
- Scope: Part of the image.
- Persistence: Persistent across all containers started from the image.
Example:
```
ENV NODE_ENV=production
```
### Passing Environment Variables at Runtime:

- Scope: Specific to the container instance.
- Persistence: Only for the lifetime of the container.
Examples:
Using ENV in docker run:
```
docker run -e NODE_ENV=production -e API_KEY=your_api_key your_image_name
```
Using docker-compose.yml:
```
environment:
  - NODE_ENV=production
  - API_KEY=your_api_key
```
## Viewing Docker Logs

```
docker service logs --follow [SERVICE_NAME | SERVICE_ID]
```
```
sudo docker service ps [container id]
```
Going inside docker container
```
sudo docker exec -it 3ff092d5d84b  bash
```

---
---

## Run The Docker Compose File
```
sudo docker stack deploy -c docker-compose.yml swarmnodeapp
```
### Or Run In Debug Mood
```
sudo docker stack deploy -c docker-compose.yml  -e DEBUG="app:*"  swarmnodeapp
```
## Kafka And ZooKeeper
Image:
```
version: ‘3.7'

services:
  zookeeper:
    image: confluentinc/cp-zookeeper:latest
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
      ZOOKEEPER_TICK_TIME: 2000
    ports:
      - 2181:2181
    
  kafka:
    image: confluentinc/cp-kafka:latest
    depends_on:
      - zookeeper
    ports:
      - 9092:9092
      - 29092:29092
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://kafka:9092,PLAINTEXT_HOST://localhost:29092
      KAFKA_LISTENER_SECURITY_PROTOCOL_MAP: PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT
      KAFKA_INTER_BROKER_LISTENER_NAME: PLAINTEXT
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
```
### Start Kafka Server
Go where docker file exist and run:
```
docker-compose up -d
```
output:
```
Creating network "kafka_default" with the default driver
Creating kafka_zookeeper ... done
Creating kafka_kafka     ... done
```
