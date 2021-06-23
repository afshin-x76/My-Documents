# Docker

Docker is an open platform for developing, shipping, and running applications.Docker enables you to separate your applications from your infrastructure.

Docker package and run an application in a isolated environment called a container.Containers are lightweight and contain everything needed to run the application.

## What can I use Docker for?

Containers are great for continuous integration and continuous delivery (CI/CD) workflows.

###### Responsive deployment and scaling
###### Running more workloads on the same hardware

## Docker architecture

Docker uses a client-server architecture. client talks to daemon, which does the  building, running, and distributing your containers. client and daemon can run on the same system, or you can connect client to a remote daemon. client and daemon communicate using a REST API, over UNIX sockets or a network interface. Another Docker client is Docker Compose, that lets you work with applications consisting of a set of containers.

![image](https://docs.docker.com/engine/images/architecture.svg)

### The Docker daemon

The Docker daemon (dockerd) listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes. A daemon can also communicate with other daemons to manage Docker services.

### The Docker client

The Docker client (docker) is the primary way that many Docker users interact with Docker. 

### Docker registries

A Docker registry stores Docker images. Docker Hub is a public registry that anyone can use, and Docker is configured to look for images on Docker Hub by default. You can even run your own private registry.

### Docker objects

When you use Docker, you are creating and using images, containers, networks, volumes, plugins, and other objects.

#### images

An image is a read-only template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization.

#### Containers

A container is a runnable instance of an image. You can create, start, stop, move, or delete a container using the Docker API or CLI. You can connect a container to one or more networks, attach storage to it, or even create a new image based on its current state.


