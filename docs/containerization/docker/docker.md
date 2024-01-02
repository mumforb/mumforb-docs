# Docker

Why Docker? A traditional shipping methodology might involve a lot of hope and work to sync up environments all the way through. If, however, you are able to specify the container in which the code itself runs, then it will really clean up and universalize every step along the way. It solves any kind of "works on my machine" type problems that happen between each environment, from development to production.

It also gives the ability to totally redo environments from scratch quickly. It onboards devs more quickly as well, as their environments can be spun up reliably much more quickly.

## Image

A layered file system including your code, server code, and everything else. Layered filesystems, a stack of books. An image, once run, is a container. Containers are the run-time version that can be run, started, stopped, moved, deleted, and re-run. Keep in mind that images are pegged to their version, and are therefore immutable. You don't change an image, you release/deploy a new one.

Visualize layers in an image, like a layer cake. Some or all may be immutable. The container layer is a thin read/write layer on top of the immutable image layers below.

So, to run a container with an open port accessible from the host:
`docker run -p <externalPort>:<internalPort> <imageName>`
`docker run -p <externalPort>:<internalPort> -d <imageName>` # this runs it without taking over the console

## Troubleshooting

docker logs <containerId>

## Docker Desktop

The default. Comes with everything, including a UI (at least in Windows and Mac).

- docker ps -a - list all running containers
- docker exec -it <containerId> sh # shell into a specific container
- docker rmi <imageId>
- docker images

