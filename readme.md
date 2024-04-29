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
- [Basic Terminology]()
  - [Image](#image)
  - [Container](#container)
  - [Dockerfile](#dockerfile)
- [Installation Commands on Ubuntu](#installation-on-ubuntu)
- [Docker Login Commands]()
- [DOCKERFILE]()
- [Image]()
  - [Image Management Commands]()
  - [CUSTOM IMAGES:]()
  - [Image Transfer Commands]()
- [Docker Hub Commands]()
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
## Basic Terminology
### Image
A read-only template used to create containers. Images are built from Dockerfiles and can be shared and reused across different environments.
### Container:
A runnable instance of an image. Containers run applications and contain everything needed to run the application, including the code, runtime, libraries, and dependencies.
### Dockerfile: 
A text document that contains instructions for building a Docker image. Dockerfiles define the environment inside a container, including the base image, dependencies, and runtime configuration.

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

