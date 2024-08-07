# Docker

docker is a containerization platform that allows you to run applications in isolated environments called containers. docker is a popular tool for developers and system administrators to build, ship, and run applications in any environment.

Comands to manage docker containers

```bash

docker ps # list all running containers
docker ps -a # list all containers
docker ps -q # list all container ids
docker ps -aq # list all container ids
docker stop $(docker ps -aq) # stop all running containers
docker rm $(docker ps -aq) # remove all containers
docker rmi $(docker images -q) # remove all images
docker volume ls # list all volumes
docker volume rm $(docker volume ls -q) # remove all volumes
docker network ls # list all networks
docker network rm $(docker network ls -q) # remove all networks
docker system prune # remove all stopped containers, all networks not used by at least one container, all dangling images, and all build cache
docker system prune -a # remove all stopped containers, all networks not used by at least one container, all images without at least one container associated to them, and all build cache
docker system prune --volumes # remove all stopped containers, all networks not used by at least one container, all dangling images, all build cache, and all volumes not used by at least one container
docker system prune -a --volumes # remove all stopped containers, all networks not used by at least one container, all images without at least one container associated to them, all build cache, and all volumes not used by at least one container

```

you can also use docker-compose to manage multiple containers

```bash

docker-compose up -d # start all containers in the background
docker-compose down # stop all containers
docker-compose down -v # stop all containers and remove volumes
docker-compose down --rmi all # stop all containers and remove images
docker-compose down -v --rmi all # stop all containers, remove volumes, and remove images

```

you can also execute commands in a running container

```bash

docker exec -it container_name_or_id bash # execute bash in a running container
docker exec -it container_name_or_id sh # execute sh in a running container
docker exec -it container_name_or_id /bin/bash # execute bash in a running container
docker exec -it container_name_or_id /bin/sh # execute sh in a running container

```

you can also copy files from and to a container

```bash

docker cp /path/to/file container_name_or_id:/path/to/destination # copy file to container
docker cp container_name_or_id:/path/to/file /path/to/destination # copy file from container

```

you can also create a docker image from a container

```bash

docker commit container_name_or_id image_name:tag # create an image from a container

```

you can also create a docker image from a Dockerfile

```bash

docker build -t image_name:tag /path/to/Dockerfile # create an image from a Dockerfile

```

you can also push and pull images from a docker registry

```bash

docker login # login to docker registry
docker push image_name:tag # push image to docker registry
docker pull image_name:tag # pull image from docker registry

```

you can also create a docker network

```bash

docker network create network_name # create a network

```

you can also create a docker volume

```bash

docker volume create volume_name # create a volume

```

you can also create a docker container

```bash

docker run -d --name container_name image_name:tag # create a container
docker run -d --name container_name -p 8080:80 image_name:tag # create a container with port mapping
docker run -d --name container_name -v volume_name:/path/in/container image_name:tag # create a container with volume mapping
docker run -d --name container_name --network network_name image_name:tag # create a container with network mapping

```

you can also start, stop, and remove a container

```bash

docker start container_name_or_id # start a container
docker stop container_name_or_id # stop a container
docker rm container_name_or_id # remove a container

```

you can also inspect a container

```bash

docker inspect container_name_or_id # inspect a container

```

you can also view logs of a container

```bash

docker logs container_name_or_id # view logs of a container

```

you can also view the processes running in a container

```bash

docker top container_name_or_id # view processes running in a container

```

you can also view the resource usage of a container

```bash

docker stats container_name_or_id # view resource usage of a container

```

you can also view the changes made to a container

```bash

docker diff container_name_or_id # view changes made to a container

```

you can also export and import a container

```bash

docker export container_name_or_id > container_name_or_id.tar # export a container
docker import container_name_or_id.tar container_name_or_id # import a container

```

you can also save and load an image

```bash

docker save -o image_name.tar image_name:tag # save an image
docker load -i image_name.tar # load an image

```

you can also inspect an image

```bash

docker image inspect image_name:tag # inspect an image

```


you can also view the history of an image

```bash

docker history image_name:tag # view history of an image

```

you can also tag an image

```bash

docker tag image_name:tag new_image_name:new_tag # tag an image

```

you can also remove an image

```bash

docker rmi image_name:tag # remove an image

```

you can also prune images

```bash

docker image prune # remove dangling images
docker image prune -a # remove all images

```


you can also view the networks

```bash

docker network ls # view networks

```

you can also inspect a network

```bash

docker network inspect network_name # inspect a network

```

you can also remove a network

```bash

docker network rm network_name # remove a network

```

you can also view the volumes

```bash

docker volume ls # view volumes

```

you can also inspect a volume

```bash

docker volume inspect volume_name # inspect a volume

```

you can also remove a volume

```bash


docker volume rm volume_name # remove a volume

```
