# Bridge Networks

Bridge networks allow communication between containers. This allows any container that has access to the network to talk to each other. 

`docker network create --driver bridge network-name` - this command creates a bridge network that allows communication between the containers. Happens at the command line using the Docker CLI. (In general, obviously, this will be set up via Docker Compose, but this just demonstrates the capability of doing it manually.)

`docker run -d -p 3000:3000 --net=network-name --name=mongodb mongo` - this runs a Docker image and attaches it to the listed network.

Then you add additional containers to the same network one at a time, and making sure each container has a unique name. Then, by passing in config, you can sync up paths and hostnames, allowing for container-to-container communication that is predictable and scripted.