# Docker Custom Image Creation and Usage Guide

This guide will walk you through creating a custom Dockerfile, building a Docker image from it, and running a container from the built image.

## Prerequisites

- Docker installed on your machine. You can download and install Docker from [here](https://www.docker.com/products/docker-desktop).

## Step 1: Create a Dockerfile

A Dockerfile is a text document that contains all the commands to assemble an image. Here's an example of a simple Dockerfile for a Node.js application.

1. Create a directory for your project and navigate into it:

    ```sh
    mkdir my-node-app
    cd my-node-app
    ```

2. Create a file named `Dockerfile` (without any extension) and open it in a text editor:

    ```Dockerfile
    # Use an official Node.js runtime as a parent image
    FROM node:14

    # Set the working directory in the container
    WORKDIR /usr/src/app

    # Copy the package.json and package-lock.json files to the working directory
    COPY package*.json ./

    # Install the dependencies
    RUN npm install

    # Copy the rest of the application code to the working directory
    COPY . .

    # Expose port 8080 to the outside world
    EXPOSE 8080

    # Define the command to run the application
    CMD ["node", "app.js"]
    ```

3. Create a simple `package.json` file for a Node.js application:

    ```json
    {
      "name": "my-node-app",
      "version": "1.0.0",
      "description": "A simple Node.js application",
      "main": "app.js",
      "scripts": {
        "start": "node app.js"
      },
      "dependencies": {
        "express": "^4.17.1"
      }
    }
    ```

4. Create a simple `app.js` file:

    ```javascript
    const express = require('express');
    const app = express();
    const port = 8080;

    app.get('/', (req, res) => {
      res.send('Hello World!');
    });

    app.listen(port, () => {
      console.log(`App running on http://localhost:${port}`);
    });
    ```

## Step 2: Build the Docker Image

To build the Docker image from the Dockerfile, run the following command in the directory containing the Dockerfile:

```sh
docker build -t my-node-app .
