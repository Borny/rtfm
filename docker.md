# Docker

- About
- Install
- Images
- Containers
- Dockerfile
- Docker CLI
- Volumes
- Docker Ignore
- ENVironment
- ARGuments
- Network
- Compose

## About

**Docker** makes it possible to use any code/app in any environment by using images/containers that hold all the code.

## Install

## Images

An **Image** is like a package that holds the **code** aswell as the **environment** to run that code. They are like blueprints.

## Containers

**Containers** are the instance of an **Image**. There can be multiple Containers running based on the same Image. Containers run and execute the code based on the **Images**

## Dockerfile

Config file at the root of the project. It gives instructions to Docker to set up the image.

```Dockerfile
FROM node:16 => name of the image

WORKDIR /app => where the image/container will be stored

COPY package.json /app => will copy the package.json into the /app folder

RUN npm install => will run the command inside the working directory (/app)

COPY . /app => will copy all the code inside the /app folder

EXPOSE 3000 => will expose the port outside the container

CMD ["node", "app.js"] => will run this command

```

## Docker CLI

- build
- run
- ps
- images
- stop
- start
- attach
- detach
- logs
- interact
- rm
- rmi
- prune
- inspect
- cp

### build

Creating an **Image**:  
`docker build .`

Add **-t [customName]:[customTag]** to add a tag to the Image:  
`docker build -t [customName]:[customTag] .`

### run

Will create and run a new **Container**:  
`docker run -p [localPort]:[exposedPort] [imageId]`  
By default it will run in **attach** mode.

Add **--name** to add a name to a Container:  
`docker run -p --name [customName] [localPort]:[exposedPort] [imageId]`

### ps

Listing the processes docker created.

Running processes:  
`docker ps `

All processes:  
`docker ps -a`

### images

List all **Images**
`docker images`

### stop

Stops a Container:  
`docker stop [containerName]`

### start

Will run an existing Container. By default it will run in **detach** mode:  
`docker start [containerName]`

Add **-a** to start in attach mode:  
`docker start -a [containerName]`

### attach

Attach to a running Container:  
`docker container attach [containerName]`

### detach

Will run a Container in detach mode:  
`docker run -d [imageId]`

### logs

Will log all the passed output from the Container:  
`docker logs [containerName]`

Add **-f** (follow) to listen to future outputs:  
`docker logs -f [containerName]`

### interact

Interact with the code inside the Container:
-i => interact
-t => will open a terminal  
`docker run -it [imageId]`
_or in start mode_
`docker start -ai [containerName]`

### rm

Will remove the desired stopped Containers (a running Container cannot be removed):  
`docker rm [containerName]`

Add **--rm** to removing automatically when a Container is stopped:  
`docker run --rm [imageId]`

### rmi

Removing an Image:  
`docker rmi [imageId]`

### prune

Will remove **ALL** Containers:  
`docker container prune`

Will remove all Images without tags:  
`docker image prune`  
An Image cannot be removed if an instance exists even if they are not running, all the Containers from that Image must be removed first.
Will remove **ALL** Images:  
`docker image prune -a`

### inspect

Will print details of an Image:  
`docker image inspect [imageId]`

### cp

Copying files to and from a Container:  
To: `docker cp path/to/copy [containerName]:target/path`
From: `docker cp [containerName]:container/path target/path `

## Volumes

- About
- Anonymous Volumes
- Named Volumes
- Bind Mount Volumes

### About

Volumes are folders on the host machine mounted (made available/mapped) into Containers

`docker volume ls` => will list all the available Volumes

### Anonymous Volumes

Anonymous Volumes are removed when a Container is removed.

### Named Volumes

Named Volumes will persist even when the Container is removed.
`docker run --rm -v [someVolumeName]:/path/to/volume` => e.g: `docker run --rm -v feedback:/app/feedback.html some-image-name:with-a-tag`

### Bind Mount

It is a specified path on the host machine which is mapped to some container-internal path. It is usefull when providing live data to the Container as no **rebuild** is needed.

### Read Only Volumes

Add **:ro** to a Volume to make it Read-Only: `docker run --rm -v feedback:/app/feedback.html:ro some-image-name:with-a-tag`

## Docker Ignore

Ignore files or folders by creating a **.dockerignore** file and adding in what should not be copied. Basically anything that is not required for the App to run in production mode:

```
node_modules
Dockerfile
.git
```

## ENVironment

Use **environment variables** in the Dockerfile:

```
ENV key value => e.g: ENV PORT 80
```

Override the ENV variable in the Run command:

`docker run --env (or -e) [key] [value]`

## ARGuments

```
ARG DEFAULT_PORT=80

ENV PORT $DEFAULT_PORT
```

## Network

- Outside communication
- Same local network
- Cross Container

### Outside communication (www)

It is possible to make http request by default with no additional settings.

### Same local network

**localhost** needs to be replaced by **host.docker.internal**

### Cross Container

- Container IP
- Container name

#### Container IP

- Find the Container IP with: `docker container inspect [containerName]` under the field: "IPAdrress"
- Replace **localhost** with the IP

#### Container name

- First create a network: `docker network create [some-network-name]`
- Then add it in the **run** command: `docker run --network [some-network-name] --name container-name image-name`
- Then update the URL: `http://container-name:......`

## Compose

Tool that merges commands (build, run, ...) in a config docker-compose.yml file:

```dockerfile
version: "3.8"
services:
  mongodb:
    image: 'mongo'
    volumes:
      - data:/data/db
    # environment:
    #   MONGO_INITDB_ROOT_USERNAME: max
    #   MONGO_INITDB_ROOT_PASSWORD: secret
      # - MONGO_INITDB_ROOT_USERNAME=max
    env_file:
      - ./env/mongo.env
  backend:
    build: ./backend
    # build:
    #   context: ./backend
    #   dockerfile: Dockerfile
    #   args:
    #     some-arg: 1
    ports:
      - '80:80'
    volumes:
      - logs:/app/logs
      - ./backend:/app
      - /app/node_modules
    env_file:
      - ./env/backend.env
    depends_on:
      - mongodb
  frontend:
    build: ./frontend
    ports:
      - '3000:3000'
    volumes:
      - ./frontend/src:/app/src
    stdin_open: true
    tty: true
    depends_on:
      - backend

volumes:
  data:
  logs:
```

### docker-compose up

Run `docker-compose up` to build the images, create the network and run the containers
Add `-d` for detach mode

### docker-compose down

Run `docker-compose down` to remove everything but volumes
Add `-v` to remove volumes
