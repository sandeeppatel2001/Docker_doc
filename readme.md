# DOCKER DOCUMENTATION
Docker simplifies application creation, deployment, and running through containers, providing a convenient utility for packaging and running applications in a semi-isolated environment.


### References.
- gitlab.com
- Books: "Docker Deep Dive" by Nigel Poulton, "The Docker Book" by James Turnbull
- Online Courses: Docker Mastery on Udemy, Docker Deep Dive on Pluralsight


----

## Version History.

- Version: 1.0, Author: Sandeep Patel, Last Updated: April 29th, 2024, Created: April 17th, 2024.

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
- [6.DOCKER COMPOSE:](#docker-compose)
- [Docker Swarm Commands](#docker-swarm)
- [Scaling Containers](#scaling-containers)
- [Viewing Docker Logs:](#viewing-docker-logs)
- [MONITORING AND LOGGING:]()


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
- Another way
- Starting Containers
```
docker container start nginx
```
Stopping Containers
```
docker container stop nginx
```
Restarting Containers
```
docker container restart nginx
```
Pausing Containers
```
docker container pause nginx
```
Unpausing Containers
```
docker container unpause nginx
```
Blocking a Container
```
docker container wait nginx
```
Sending SIGKILL Containers
```
docker container kill nginx
```
Check the Containers
```
docker ps
```
To see all running containers
```
docker container ls
```
Container Logs
```
docker logs infinite
```
‘tail -f’ Containers’ Logs
```
docker container logs infinite -f
```
Inspecting Containers
```
docker container inspect infinite
```
Container Resource Usage
```
docker container stats infinite
```
inspecting changes to files or directories on a container’s filesystem
```
docker container diff infinite
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
version: '3.8'

services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
    volumes:
      - ./html:/usr/share/nginx/html

  database:
    image: postgres:latest
    environment:
      POSTGRES_DB: example_db
      POSTGRES_USER: example_user
      POSTGRES_PASSWORD: example_password
    volumes:
      - db_data:/var/lib/postgresql/data

volumes:
  db_data:

```
Start your application:
```
docker-compose up
```
## Docker Swarm
Docker Swarm is Docker's native clustering and orchestration tool. It allows you to manage a cluster of Docker nodes as a single virtual system. Docker Swarm turns a pool of Docker hosts into a single, virtual Docker host. This enables you to scale your applications horizontally by adding more nodes to the cluster and distributing containers across these nodes.

```
version: '3.8'
services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
    deploy:
      replicas: 3
      update_config:
        parallelism: 2
        delay: 10s
      restart_policy:
        condition: on-failure
  database:
    image: postgres:latest
    environment:
      POSTGRES_DB: example_db
      POSTGRES_USER: example_user
      POSTGRES_PASSWORD: example_password
    volumes:
      - db_data:/var/lib/postgresql/data

volumes:
  db_data:

```
## Scaling Containers
replicas: Specifies the number of container instances to run for the service.
### Deploy the stack:
```
docker stack deploy -c docker-compose.yml my_stack
```
## Viewing Docker Logs
```
docker service logs --follow [SERVICE_NAME | SERVICE_ID]
```
```
sudo docker service ps [container id]
```
Or can go inside docker container
```
sudo docker exec -it 3ff092d5d84b  bash
```
---
---


