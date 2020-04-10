
## useful commands


[https://goinbigdata.com/docker-run-vs-cmd-vs-entrypoint/]


`docker run -it <image>`
`docker ps −a` shows all running docker container on a client

TODO: [https://bitbucket.org/crowdsalat/security_lab_vpn/src/master/]
TODO: B5 PSE Vortrag

## Dockerfile

[Docker file reference](https://docs.docker.com/engine/reference/builder/)
[Nice summary of run, cmd and entypoint](https://goinbigdata.com/docker-run-vs-cmd-vs-entrypoint/)

### Create image form dockerfile

- `docker build <context>`  builds Docker images from a Dockerfile and a 'context'.
- context is the set of files located in the specified PATH or URL
- the context is processed recursively
- `docker build .` is used to build a image based on a dockerfile in the current directory.
- use `-t <tageName>` to add a tag to the image


### shell form vs. exec form

- the forms can be distinguished by their syntax. 
- they differ in their behavior.
- some docker commands like RUN, ENTRYPOINT or CMD can be used in either form.

shell form: 

- syntax: `<instruction> <command>`
- like calling `/bin/sh -c <command>`
- variable substitution works e.g. `exec echo $path` 

exec form:

- syntax: `<instruction> ["executable", "arg1", "arg2", ...]`
- like calling: `executable` directly, so it must be a valid path (/bin/echo instead of echo)
- preferred for CMD and ENTRYPOINT

### dockerfile commands

RUN, CMD and Entrypoint all execute a command, but:

- **RUN** creates a new layer on the union file system
- **CMD** sets default command to run when container starts. This command can be replaced by another command by calling: `docker run -it <image> <command which is used instead>`
- **ENTRYPOINT** sets default command to run when container starts, but cannot be overwritten (exec and shell form). You can add a CMD without a command/executable afterwards to add further arguments to the entrypoint command which can be overwritten (only in exec form).

--> Use RUN for new layer and prefer ENTRYPOINT over CMD if you do not need to change arguments.

## connect containers via user-defined networks

## volumes
