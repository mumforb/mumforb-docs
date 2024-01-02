# Dockerfile

A test document that contains all the commands a user could call on the command line.

Dockerfile >> `docker build` >> Container

Common commands in a Dockerfile.
```
FROM node:alpine
LABEL author="Me"
ENV NODE_ENV=production
WORKDIR /var/www # you are here once the container starts running
COPY . . # where to copy from, and where to copy to, and launch
RUN npm install # anything you want to run at launch
EXPOSE 3000 # port number it's going to be listening on
ENTRYPOINT ["node", "server.js"] # first command to run to start the executable/starting point
```

## Tips

Docker works with layers, so an efficient way to mount all the npm code into a Docker image might be:

```
...
WORKDIR /var/www
COPY package*.json # wildcard gets both package.json and package-lock.json
RUN npm ci

COPY . ./
```
This is particularly powerful along with a `.dockerignore`.


## Deploy

`docker build -t <registry>/<name>:<version> .`
`docker push <registry>/<name>:<version>`

Versioning is really, really important.

## VS Code Docker Extension