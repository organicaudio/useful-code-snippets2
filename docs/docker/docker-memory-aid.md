# docker memory aid

 [vs code plugin for docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker) is pretty nice.

## docker cli useful commands

official overview of [docker cli commands](https://docs.docker.com/engine/reference/commandline/docker/).

- `docker build . -t crowdsalat/tag-name` creates a image based on a Dockerfile and the context/ working directory and subdirectories
- `docker run <image>` starts a container from an image
    - `-d` run as daemon
    - `-it` runs interactively so you can execute commands in container (-t Allocate a pseudo-tty, -i Keep STDIN open )
    - `-p 1000:1000` maps a port in container on a port on client
    - `--mount` TODO
    - `-v` TODO
    - `-e VAR1=bla -e VAR2=blubb` defines environmental variables in the container
    - `—rm` remove container after it is stopped.
    - `—name` add name to the container.
- `docker exec -it container_name bash` connects to a running container with the name container_name
- `docker ps −a` shows all running docker container on a client
- `docker image / container / network / volumes / system`
      - ls
      - rm
      - inspect
- `docker system prune` removes:
    1. all stopped containers (`docker container prune`)
    2. all networks not used by at least one container (`docker network prune`)
    3. all dangling images (`docker image prune`)
    4. all dangling build cache
- `docker network create --driver bridge <bridgeName>` create a new bridge network
- `docker volume create <volumeName>` create a new bridge volume

## Dockerfile

[Docker file reference](https://docs.docker.com/engine/reference/builder/)
[Nice summary of run, cmd and entypoint](https://goinbigdata.com/docker-run-vs-cmd-vs-entrypoint/)

### Create image form dockerfile

- `docker build <context>`  builds Docker images from a Dockerfile and a 'context'.
- context is the set of files located in the specified PATH or URL
- the context is processed recursively
- `docker build .` is used to build a image based on a dockerfile in the current directory.
- use `-t <tageName>` to add a tag to the image

Simplistic docker file for java application:

```dockerfile
FROM openjdk:8-jdk-alpine
COPY target/*.jar app.jar
ENTRYPOINT["java", "-jar", "app.jar"]
```

### shell form vs. exec form

- the forms can be distinguished by their syntax. 
- they differ in their behavior.
- some docker commands like RUN, ENTRYPOINT or CMD can be used in either form.

shell form: 

- syntax: `<instruction> <command>`
- like calling `/bin/sh -c <command>`
- variable substitution works e.g. `exec echo $path`
- application which is started this way will not receiving interrupt signals (like crlt + c)

exec form:

- syntax: `<instruction> ["executable", "arg1", "arg2", ...]`
- like calling: `executable` directly, so it must be a valid path (/bin/echo instead of echo)
- preferred for CMD and ENTRYPOINT
- no variable replacement ($PATH does not work)

### dockerfile commands

RUN, CMD and Entrypoint all execute a command, but:

- **RUN** creates a new layer on the union file system
- **CMD** sets default command to run when container starts. This command can be replaced by another command by calling: `docker run -it <image> <command which is used instead>`
- **ENTRYPOINT** sets default command to run when container starts, but cannot be overwritten (exec and shell form). You can add a CMD without a command/executable afterwards to add further arguments to the entrypoint command which can be overwritten (only in exec form).

--> Use RUN for new layer and prefer ENTRYPOINT over CMD if you do not need to change arguments.

COPY and ADD copy files to an image and create a new layer, but:

- **COPY** can only be used on local files
- **ADD** can download any URL and extract tar balls

--> use COPY for local files

- **ENV** sets a environment variable (`ENV path=/opt/`).
- **ARG** defines parameters of a docker image which can be overridden when the images is build (e.g. `$ docker build --build-arg arg_name=blubb .`)

### useful images

A list of relatively small images. If present alpine images tend to be the smallest. Docker repositories mostly are organized via the tags (:8-jdk-alpine, :11-jdk-buster etc.).

Java:

- [openjdk:8-jdk-alpine](https://hub.docker.com/_/openjdk/)
- [openjdk:11-jdk-buster](https://hub.docker.com/_/openjdk/)
- [openjdk:14-jdk-slim](https://hub.docker.com/_/openjdk/)
- [adoptopenjdk/openjdk8-openj9:alpine](https://hub.docker.com/r/adoptopenjdk/openjdk8-openj9)
- [adoptopenjdk/openjdk11:alpine](https://hub.docker.com/r/adoptopenjdk/openjdk11)

## dockerfile vs. command line

### -p vs EXPORT

EXPORT does not actually publish the given port. It is a documentation for the user. The user of a image needs to export the port via -p flag when running docker run.

### --volume vs. VOLUME

VOLUME creates a anonymous volume even when docker run does not specify a --volume parameter.

## user amd groups in container

TODO general information on best practice

To add user and groups to containers:

[Source](https://github.com/mhart/alpine-node/issues/48)

```dockerfile
# ubuntu as base image
RUN groupadd -r app && useradd -r app -g app 

# alpine as base image
RUN addgroup -S app && adduser -S app -Gapp 
```

To check if commands exists on a base image and which parameters they have just call: 

```shell
# docker run <baseimage:tag> <command>
docker run alpine:3.6 adduser

# alternativly
# docker run <baseimage:tag> <command> -?
docker run alpine:3.6 adduser -?
```
