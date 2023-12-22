# Docker

[Docker](https://www.docker.com/) is the primary containerization software used today. While it is different from a Virtual Machine, in terms of how it shares (or doesn't share) resources with the host computer, it can be thought of as a computer-inside-a-computer. Docker images can be extremely lightweight (a very popular implementation, Alpine Linux, is only 5 megabytes), and as such, a relatively modest modern host computer can run many simultaneous Docker instances. Containerizing individual applications in Docker provides consistency and portability, and is a significant player in modern software development.

A Docker container typically consists of all the files required to run a specific application, along with building it. The base image to run in the instance, along with all the other startup build tasks, are defined in a `Dockerfile`.

## CI/CD

By providing an easy to build, consistent platform, Docker enables CI/CD pipelines to quickly create new instances of their application for Unit Testing, Integration Testing, deployment, and other tasks.

## Kubernetes

[Kubernetes](./kubernetes.md) works very well with Docker containers - in fact, in our context, Kubernetes is, at its core, a fleet of many Docker containers. By putting our applications inside Docker containers, we enable rapid, consistent deployment into Kubernetes clusters, with assurance that the application will run in many pods the way we expect it to.

## Dev Containers in VSCode

See [this section](vscode.md#dev-container) for more.
