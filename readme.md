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
- [General Docker Commands]()
- [CONTAINER MANAGEMENT:]()
- [Docker Image Management Commands]()
- [5. DOCKER NETWORKING:]()
  - [Bridge:]()
  - [Host:]()
  - [Overlay:]()
- [Docker Exposing Ports Commands]()
- [6.DOCKER COMPOSE:]()
- [Docker Swarm Commands]()
- [Docker file Commands]()
- [Docker Volume Commands]()
- [Docker CP commands]()
- [Scaling Containers]()
- [MONITORING AND LOGGING:]()
- [Viewing Docker Logs:]()


----


## INTRODUCTION TO DOCKER:
Docker is a platform for developing, shipping, and running applications inside containers. Containers allow developers to package an application and its dependencies into a standardized unit for software development, making it easy to deploy applications across different environments.
(sub contents must be started with triple # )

### [Installation on Ubuntu]
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
This command builds a Docker image named my-node-app based on the instructions in the Dockerfile.
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

