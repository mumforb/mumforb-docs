# VSCode

VSCode is developed my Microsoft and is valued for it's relatively light weight, speed, and tremendous flexibility and extensibility.

## Dev Container

One of the most powerful extensions in VSCode is the Dev Container (also called a Remote Container). A VSCode Dev Container is an instance of VSCode that can be opened and run inside an individual [Docker](./docker.md) container. The contents and configuration of this container is defined by files in the `.devcontainer` directory that ships with the repository. The `devcontainer.json` file is specific to VSCode, and defines a wide variety of options for the Dev Container. There is also the option of a standard `Dockerfile` which the `devcontainer.json` defines.

Most (if not all) of the Dev Containers in use start with an extremely lightweight Linux instance, either the well-known [Alpine](https://hub.docker.com/_/alpine) distribution, or another distribution that starts with Alpine but adds additional software. (For example, when you run The Oracle's home repository locally, you will be starting with [Microsoft's official Python 3 image](https://github.com/microsoft/vscode-dev-containers/tree/main/containers/python-3), which is based on Debian Linux.) Then, the `Dockerfile` and `devcontainer.json` files will install additional software and configuration. If something goes amiss, you can always reset a Dev Container. (There are several options available when using a running container available by clicking on blue Dev Container button in the bottom left-hand corner of VSCode.)

### Running a Dev Container

Running a Dev Container is typically just a matter of seeing notification that appears in the bottom right-hand corner of VSCode when you open a repository that features the definition files for container. If you miss that notification, the blue icon on the bottom right-hand corner should offer the option of reopening in a container.

### Rebuilding a Dev Container

If something goes wrong with your project, a good option is to rebuild your Dev Container. While this doesn't clear the cache (more on that in a moment), it will totally destroy and rebuild a new Docker container, which will often clear up issues. Either click on the Dev Container menu in the bottom left-hand corner and select `Rebuild Container` or select `cmd + shift + P` (Mac) or `ctrl + shift + p` (Windows) and type `Remote-Containers: Rebuild Container`.

### Rebuilding a Dev Container and Destroying Cache

To also destroy the cache while rebuilding a Dev Container, select `cmd + shift + P` (Mac) or `ctrl + shift + p` (Windows) and type `Remote-Containers: Rebuild Container Without Cache`. This will take appreciably longer but can help clear up stubborn issues with dependencies.
