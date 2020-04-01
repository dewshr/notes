# Docker

- [Docker](#docker)
  - [Installing Docker](#installing-docker)
  - [Getting Docker Images](#getting-docker-images)
  - [Running Containers](#running-containers)
  - [Listing Containers in Docker](#listing-containers-in-docker)
  - [Clearing Non-running Containers](#clearing-non-running-containers)
  - [Mounting a Host Filesystem](#mounting-a-host-filesystem)
    - [Fix for permissions issues:](#fix-for-permissions-issues)
  - [Creating a Docker Image](#creating-a-docker-image)
    - [By modifying an existing image](#by-modifying-an-existing-image)
    - [Using a docker file](#using-a-docker-file)
  - [Docker Compose](#docker-compose)

## Installing Docker

    sudo dnf install docker
    sudo sudo systemctl enable docker.service
    sudo systemctl start docker.service

**Note:** Fedora 31 had problems starting the docker daemon. 

Debugging with: `sudo dockerd --debug` shows "Devices cgroup isn't mounted". The solution was to modify the grub in order to start cgroup


    sudo dnf install -y grubby
    sudo grubby --update-kernel=ALL --args="systemd.unified_cgroup_hierarchy=0"

Reboot computer.

Finally, add user to the Docker group:
    
    sudo groupadd docker && sudo gpasswd -a ${USER} docker

Log out and in again to see changes.


## Getting Docker Images

Pull from Docker Hub:

    docker pull hub.docker.com/r/microsoft/powershell

Search for images: `docker search`

See installed images: `docker images`

## Running Containers

Use `docker run` to run an application inside a container.

    -d       run in background (daemonize)
    -i       interactive
    -t       tty
    --name   give container a name
    --rm     remove container when it exits

Start a container (can use an assigned name): `docker start`

Stop a container: `docker stop`

execute a command off a running container: `docker exec`

Look inside container: `docker logs`

## Listing Containers in Docker

To show only running containers use:

    docker container ls

    Options:
    -a    see all containers
    -s    see container sizes

## Clearing Non-running Containers

Containers that are not running are not taking any system resources besides disk space. 

The docker run flag `--rm` will automatically remove a container when it exits.

Remove a specific container: `docker rm 3e552code34a`

Removes all stopped containers: `docker container prune`

Clean unused containers, networks, images, and volumes: `docker system prune`

## Mounting a Host Filesystem 

Use the `-v` flag:

    docker run -v /host/directory:/container/directory -other -options image_name command_to_run

### Fix for permissions issues:

SELinux can cause issues with mounting volumes
The solution is to issue a SELinux rule: 
`chcon -Rt svirt_sandbox_file_t /path/to/volume`



This got easier recently since Docker finally merged a patch which will be showing up in docker-1.7 (We have been carrying the patch in docker-1.6 on RHEL, CentOS, and Fedora).

This patch adds support for "z" and "Z" as options on the volume mounts (-v).

For example:

    docker run -v /var/db:/var/db:z rhel7 /bin/sh

Will automatically do the `chcon -Rt svirt_sandbox_file_t /var/db` described in the man page.

Even better, you can use Z.

    docker run -v /var/db:/var/db:Z rhel7 /bin/sh

This will label the content inside the container with the exact MCS label that the container will run with, basically it runs `chcon -Rt svirt_sandbox_file_t -l s0:c1,c2 /var/db`,
where s0:c1,c2 differs for each container.

https://docs.docker.com/storage/volumes/

E.g. to run my powershell docker container with a mounted home directory:

```
docker run -v /home/ravila/:/home/ravila/:Z -it mcr.microsoft.com/powershell
```

## Creating a Docker Image

### By modifying an existing image

If you change anything (like install new packages) in the running container and exit the container the changes are not automatically saved. If you want to save them in an image, use docker commit.

### Using a docker file

A Dockerfile content can be as simple as:

    FROM fedora:latest
    CMD env

Docker file reference: https://docs.docker.com/engine/reference/builder/

Best practices: https://docs.docker.com/develop/develop-images/dockerfile_best-practices/


In a directory with a Dockerfile run:

    sudo docker build -t "my-image" .

If the build is successful you can see `my-image` in `docker images` output.

## Docker Compose

https://docs.docker.com/compose/

https://developer.fedoraproject.org/tools/docker/compose.html


Docker Compose is a tool to orchestrate Docker containers using a simple YAML file which describes your whole setup.

    sudo dnf install docker-compose

