# 2. DOCKER COMMANDS

* `docker + double tab` → shows the list of available Docker commands.
* `docker command --help` → shows what operations/options can be performed using that command.

Example:

```bash
docker run --help
```

Docker command structure:

```bash
docker command operation
```

Example:

```bash
docker system df
```

---

# docker -v

Used to know the Docker version.

```bash
docker -v
```

---

# docker system df

Used to check Docker disk usage.

```bash
docker system df
```

---

# docker system prune

Used to remove unused Docker resources such as stopped containers, unused networks, dangling images and build cache.

```bash
docker system prune
```

---

# docker stats

Used to get real-time resource usage of containers.

```bash
docker stats
```

---

# docker search imageName

Used to search for public images in Docker Hub.

```bash
docker search mysql
```

---

# docker images

Used to list the images available in the local Docker host.

```bash
docker images
```

---

# docker pull

Used to download an image from a Docker registry to the local host.

```bash
docker pull mysql
```

---

# docker create & docker run

Both commands are used to create a container from an image.

`docker create`:

* Creates the container.
* Does not start the container.

```bash
docker create --name mysql-db mysql:latest
```

`docker run`:

* Creates the container.
* Starts the container.

```bash
docker run --name mysql-db mysql:latest
```

By default, `docker run` attaches the terminal to the container.

To run the container in the background:

```bash
docker run --name mysql-db -d mysql:latest
```

`-d` → runs the container in detached/background mode.

---

# docker ps

Shows currently running containers.

```bash
docker ps
```

---

# docker ps -a

Shows all containers, including stopped containers.

```bash
docker ps -a
```

---

# docker stop

Used to stop a running container.

```bash
docker stop containerName
```

Example:

```bash
docker stop mysql-db
```

---

# docker start

Used to start an existing stopped container.

```bash
docker start containerName
```

---

# docker rm

Used to remove a container.

```bash
docker rm containerName
```

---

# docker rmi

Used to remove an image from the local Docker host.

```bash
docker rmi imageName
```

---

# docker exec

Used to execute commands inside a running container.

To enter the container shell:

```bash
docker exec -it containerName /bin/sh
```

or:

```bash
docker exec -it containerName /bin/bash
```

`-i` → interactive
`-t` → terminal

We can also execute a command without entering the container.

Example:

```bash
docker exec containerName uname -a
```

---

# docker cp

Used to copy files between the Docker host and container.

It works somewhat similar to `scp`.

Host → Container:

```bash
docker cp test.txt container1:/tmp/
```

Container → Host:

```bash
docker cp container1:/tmp/test.txt /tmp/
```

---

# docker logs

Used to get logs from a container.

```bash
docker logs containerName
```

To continuously watch the logs:

```bash
docker logs -f containerName
```

---

# docker login

Used to login to a Docker registry.

```bash
docker login
```

---

# 3. DOCKER IMAGES

* Docker image is a read-only blueprint/template for creating containers.
* It contains multiple layers.
* Dockerfile instructions such as `RUN`, `COPY`, etc. can create image layers.

---

# docker search

Used to search for public images.

```bash
docker search mysql
```

---

# docker images

Used to list images available in the local Docker host.

```bash
docker images
```

---

# docker pull

Used to download an image from a registry.

```bash
docker pull mysql
```

Example:

```bash
docker pull mysql:8.0
```

---

# docker inspect

Used to get detailed information about an image.

```bash
docker image inspect mysql
```

---

# docker history

Used to see the history/layers of an image.

```bash
docker history mysql
```

---

# docker save

Used to save an image into a `.tar` file.

```bash
docker save mysql -o mysql.tar
```

---

# docker load

Used to restore/load an image from a `.tar` file.

```bash
docker load -i mysql.tar
```

---

# docker rmi

Used to remove an image from the local Docker host.

```bash
docker rmi mysql
```

---

# docker commit

Used to create a new image from an existing container.

```bash
docker commit containerName newImageName
```

Example:

```bash
docker commit new-mysql custom-mysql
```

We can make changes inside a container and then create an image from that container.

When we create a new container using that image, the committed changes will be available.

---

# docker push

Used to push a local image to a Docker registry.

```bash
docker push sugumar/custom-mysql:1.0
```

---

# docker tag

Used to create another name/tag for an image.

```bash
docker tag mysql sugumar/custom-mysql:1.0
```

---

# 4. DOCKER IMAGE CREATION USING DOCKERFILE

Dockerfile is used to create a Docker image.

Example:

```dockerfile
FROM ubuntu:24.04

RUN apt update

RUN apt install -y nginx

COPY index.html /var/www/html/

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

Build the image:

```bash
docker build -t my-nginx .
```

---

# 5. DOCKER NETWORKS

Docker network is used for communication between containers and external systems.

## Network Types

1. `bridge`
2. `none`
3. `host`

---

# bridge network

Bridge is the default network for normal Docker containers.

Example:

```bash
docker run -d --name nginx nginx
```

To access a container service from outside the Docker host, we normally need port forwarding.

Example:

```bash
docker run -d --name nginx -p 8080:80 nginx
```

Here:

```text
8080 = Docker host port
80   = Container port
```

---

# none network

Used when the container should not have normal network connectivity.

```bash
docker run -d --network none alpine
```

---

# host network

Container uses the host's network stack.

```bash
docker run -d --network host nginx
```

With host networking, the container does not get a separate Docker network interface in the normal way.

---

# 6. DOCKER VOLUMES

Three common types:

1. Bind Mount
2. Docker Volume
3. tmpfs

---

# bind mount

Data is saved in the normal filesystem of the Docker host.

Example:

```bash
docker run -d --name containerName \
-v /host/path:/container/path \
imageName
```

Here:

```text
src = Docker host filesystem directory
dest = directory inside container
```

Example:

```bash
docker run -d --name mysql \
-v /home/sugu/mysql-data:/var/lib/mysql \
mysql
```

---

# Docker Volume

Data is stored in Docker-managed storage.

First create the volume:

```bash
docker volume create appVolume
```

Then assign it to the container:

```bash
docker run -d --name containerName \
-v appVolume:/app/data \
imageName
```

Or:

```bash
docker run -d --name containerName \
--mount source=appVolume,destination=/app/data \
imageName
```

Example for MySQL:

```bash
docker run -d --name mysql \
-v appVolume:/var/lib/mysql \
mysql
```

In both **bind mount** and **Docker volume**, the data remains even after removing the container.

The same volume can be mounted by multiple containers.

---

# tmpfs

`tmpfs` stores data temporarily in the host's RAM.

Example:

```bash
docker run -d --name containerName \
--tmpfs /app/temp \
imageName
```

The data is temporary and is not persisted like a bind mount or Docker volume.
