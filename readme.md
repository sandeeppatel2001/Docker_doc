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
- [Installation Commands]()
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

### [Installation]
```
sudo apt update
```


