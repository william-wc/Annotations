# Docker

https://docs.docker.com/engine/reference/builder/
https://www.raywenderlich.com/9159-docker-on-macos-getting-started?__s=4r3nnfjce6xekxqwxn9p
https://devhints.io/docker
https://github.com/veggiemonk/awesome-docker
https://github.com/konstruktoid/Docker/blob/master/Security/CheatSheet.adoc
https://docs.docker.com/get-started/

## Cheat sheet

```shell
# List Docker CLI commands
docker
docker container --help

# Display Docker version and info
docker --version
docker version
docker info

# Execute Docker image
docker run hello-world

# List Docker images
docker image ls

# List Docker containers (running, all, all in quiet mode)
docker container ls
docker container ls --all
docker container ls -aq

docker ps -a # List running docker containers

docker rm container-id # Remove container (can pass multiple ids)
docker rm container-name # Remove containers by name (can pass multiple names)
docker rmi image-id # Remove docker by image id
docker rmi image-name # Remove docker by image

docker commit container-id new-image-name

docker exec -it container-id # runs inside currently running container

docker rename container-name new-container-name # rename container names

docker run -it --name new-container-name image-id # runs a new container from image with specified name
```

```shell
docker search term # Searches in docker hub for any images with the term

docker search -s 5 term # Searches in docker hub for any images with the term with more than 5 stars
```

```shell
# Copies a file from inside container container-id, at the specified path, to the destination path
docker cp container-id:/bath/to/file destination
docker cp /path/to/file container-id:/another/file/path
```

```shell
# Runs a container from an image with MODE=test environment variable
# Multiple -e arguments can be passed
docker run -it -e MODE=test image-id

# Runs a container from an image with environment variables specified in `my-envs` file
docker run -it --env-file ./my-envs image-id
```

## Mounting volumes

Mounting directories

```shell
# Runs container from image image-id and mounts `~/data` dir in `/data` dir inside the container
# Must be an absolute path
docker run -it --name test1 -v ~/data:/data image-id
```

Mounting docker volumes

```shell
# Runs container from image and mounts (existing or not) a volume called `data`
# in `/data` inside the container
docker run -it --name test3 -v data:/data image-id

# shows the mounted docker volumes
docker volume ls
```

docker volumes are deattached from containers, meaning that containers can be stopped/removed while the container persists.
Multiple volumes can be attached at a time

If you create a container with multiple mounts and don't want to keep track of
all of them for every new container

```shell
# Runs a container with two volumes, `backup` and `logs`
docker run -it --name master -v backup:/backup -v logs:/logs image-id

# Runs a container with all volumes attached to container `master`
docker run -it --name slave --volumes-from master image-id
```

## Example Dockerfile

https://docs.docker.com/get-started/part2/

```Dockerfile
# Use an official Python runtime as a parent image
FROM python:2.7-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip install --trusted-host pypi.python.org -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

## Dockerfile

https://docs.docker.com/engine/reference/builder/

Format

```Dockerfile
# Comment
INSTRUCTION argument
```

A Dockerfile must start with a `FROM` instruction, specifying the *base image*

---

Environment replacements can also be used either with `$variable_name` or `${variable_name}`. The latter syntax also supports a few of the standard bash modifiers as

- `${variable:-word}` indicates that if `variable` is set then the result wil be that value. Else, then `word` will be the result
- `${variable:+word}` indicates that if `variable` is set then `word` will be the result, otherwise the result is the empty string

```Dockerfile
FROM busybox
ENV foo /bar
WORKDIR ${foo}      # WORKDIR /bar
ADD . $foo          # ADD . /bar
COPY \$foo /quux    # COPY $foo /quux
```

Environment variables are supported by the following instructions

`ADD, COPY, ENV, EXPOSE, FROM, LABEL, STOPSIGNAL, USER, VOLUME, WORKDIR, ONBUILD`

---

`.dockerignore` file is searched at root directory of the context. If it exists, the CLI modifies the context to exclude files and directories that match patterns int it.
This is useful when adding things via `ADD` or `COPY` commands.

```dockerignore
# Exclude files and directories that start with `temp` in any immediate
# subdirectory after root
# /somedir/temporary.log
# /somedir/temp
*/temp*

# Exclude files and directories that start with `temp` from any subdirectory
# that is two levels below the root
# /somedir/subdir/temporary.log
*/*/temp*

# Excludes files and directories in root directory whose names are a
# one-character extension of `temp`
# /tempa /tempb /tempc
temp?

# Excludes all files that end with `.extension` in all directories,
# including root
**/*.extension

# Lines starting with `!` can make exceptions
*.md
!README.md      # /README.md in excluded from ignoring rule
```

---

### Commands

```Dockerfile
FROM <image> [AS <name>]
FROM <image>[:<tag>] [AS <name>]
FROM <image>[@<digest>] [AS <name>]
```

The `FROM` instruction initializes a new build stage and sets the base image for subsequent instructions

- `ARG` is the only instruction that can [come before][arg-from-interaction] `FROM`
- `FROM` can appear multiple times within a single Dockerfile to create multiple images or use one build stage as a dependency for another.
- Optionally a name can be given to a new build stage by adding `AS name` to the `FROM` instruction. The name can be used in subsequent `FROM` and `COPY --from=<name|index>` instructions to refer to the image built in this stage
- `tag` or `digest` values are optional. If omitted, the builder assumes a `latest` as default

```Dockerfile
ARG     CODE_VERSION=latest
FROM    base:${CODE_VERSION}
CMD     /code/run-app

FROM    extras:${CODE_VERSION}
CMD     /code/run-extras
```

An `ARG` declared before `FROM` is outside a build stage so it can't be used in any instructions after `FROM`, to do so:

```Dockerfile
ARG     VERSION=latest
FROM    busybox:$VERSION
ARG     VERSION                         # now it can be used
RUN     echo $VERSION > image_version
```

---

```Dockerfile
# shell form, the command is run in a shell
# `/bin/sh -c` on Linux
# `cmd /S /C` on Windows
RUN <command>
RUN ["executable", "param1", "param2"]
```

The `RUN` instruction will execute any commands in a new layer on top of the current image and commit the results, the resulting commited image will be used for the next step in the `Dockerfile`.
Layering `RUN` instructions and generating commits conforms to the core concepts of Docker

> Commits are cheap and containers can be created from any point in an image's history

To use a different shell other than `/bin/sh`, use `exec` form
`RUN ["bin/bash", "-c", "echo hello"]`

- The **exec** form is parsed as a JSON array, it **requires double quotes**

---

```Dockerfile
# exec form, this is the preferred
CMD ["executable", "param1", "param2"]

# as default parameters to ENTRYPOINT
CMD ["param1", "param2"]

# shell form
CMD command param1 param2
```

There can only be one `CMD` instruction in a Dockerfile, when multiple instructions are found, only the last `CMD` will take effect.

The main purpose of `CMD` is to provide defaults for an executing container.

- If the user specifies arguments to `docker run` then they will override `CMD`
- If you'd like your container to run the same executable every time, consider the use of `ENTRYPOINT` in combination with `CMD`.

---

```Dockerfile
LABEL <key>=<value> <key>=<value> ...
```

The `LABEL` instruction adds metadata to an image, it is a key-value pair.
To include spaces within a `LABEL`, use quotes and backslashes

```Dockerfile
LABEL "com.example.vendor"="ACME Incorporated"
LABEL com.example.label-with-value="foo"
LABEL version="1.0"
LABEL description="Labels with\n
multiple lines"
```

An image can have more than one label

```Dockerfile
LABEL   multi.label1="value1" \
        multi.label2="value2" \
        other="value3"

# is equivalent to

LABEL   multi.label1="value1" multi.label2="value2" other="value3"
```

Labels included in base or parent images are inherited, if a label already exists then the most recent value is applied.

---

```Dockerfile
EXPOSE <port> [<port>/<protocol>...]
```

The `EXPOSE` instruction informs Docker that the container listens on the specified network ports at runtime.
By default, assumes `TCP` protocol

```Dockerfile
EXPOSE 80/tcp
EXPOSE 80/udp
```

You can also pass it as an argument as
`docker run -p 80:80/tcp -p 80:80/udp ...`

---

```Dockerfile
ENV <key> <value>       # places a single key-value pair
ENV <key>=<value> ...   # can place multiple key-value pairs
```

The `ENV` instruction sets the environment variable `key` to the value `value`

```Dockerfile
ENV myName John Doe
ENV myDog Rex The Dog
ENV myCat fluffy

# is equivalent to

ENV myName="John Doe" myDog=Rex\ The\ Dog \
    myCat=fluffy
```

You can view the values using `docker inspect`
And change them with `docker run --env <key>=<value>`

---

```Dockerfile
ADD [--chown=<user>:<group>] <src>... <dest>
ADD [--chown=<user>:<group>] ["<src>", ... "<dest>"]
```

The `ADD` instruction copies new files, directories or remote file URLs from `<src>` and adds them to the filesystem of the image at `<dest>`.

```Dockerfile
ADD hom* /mydir/        # adds all files starting with "hom"
ADD hom?.txt /mydir/
```

`<dest>` is an absolute path, or relative to `WORKDIR`

```Dockerfile
ADD test relativeDir/   # adds "test" to `WORKDIR`/relativeDir/
ADD test /absoluteDir/  # adds "test" to /absoluteDir/
```

All new files and directories are created with UID and GID of 0, unless `--chown` flag is specified.

```Dockerfile
ADD --chown=55:mygroup files* /somedir/
ADD --chown=bin files* /somedir/
ADD --chown=1 files* /somedir/
ADD --chown=10:11 files* /somedir/
```

- The `<src>` path must be inside the context of the build, e.g: cannot be `ADD ../something /something` because the first step of a `docker build` is to send context dir (and sub dirs) to the daemon
- If `<src>` is a URL and `<dest>` does not end with a trailing slash, then a file is downloaded from the URL and copied to `<dest>`
- If `<src>` is a URL and `<dest>` ends with trailing slash, then the filename is inferred from the URL and downloaded to `<dest>/<filename>`. For example `ADD http://example.com/foobar /` creates the file `/foobar`
- If `<src>` is a directory, the entire contents of the directory are copied, including system metadata (the directory itself is not copied, just its contents)
- If multiple `<src>` are specified (dir or wildcard) then `<dest>` must be a directory, ending with a slash
- If `<dest>` does not end with a slash, then it'll be considered a regular file and the contents of `<src>` will be written at `<dest>`
- If `<dest>` doesn't exist, it is created along with all missing directories in its path

---

```Dockerfile
ENTRYPOINT ["executable", "param1", "param2"]   # exec form, preferred
ENTRYPOINT command param1 param2                # shell form
```

An `ENTRYPOINT` allows you to configure a container that will run as an executable.
Command line arguments to `docker run <image>` will be appended after all elements in an `exec` form `ENTRYPOINT`, and will override all elements specified using `CMD`.

- shell form prevents any `CMD` or `run` command line arguments from being used
- Only the last `ENTRYPOINT` instruction in the `Dockerfile` will have an effect

```Dockerfile
FROM        ubuntu
ENTRYPOINT  ["top", "-b"]
CMD         ["-c"]
```

### How CMD and ENTRYPOINT interact

Both instructions define what command gets executed when running a container

- Dockerfile should specify at least one `CMD` or `ENTRYPOINT`
- `ENTRYPOINT` should be defined when using the container as an executable
- `CMD` should be used as a way of defining default arguments for an `ENTRYPOINT` or for executing ad-hoc command in a container
- `CMD` will be overriden when running the container with alternative arguments

---

```Dockerfile
VOLUME ["/data"]
```

The `VOLUME` instruction creates a mount point with the specified name
`VOLUME /var/log`
`VOLUME /var/log /var/db`

`docker run` command initializes the created volume with any data that exists at the specified location within the base image

- If any build steps change the data within the volume after it has been declared, those changes will be discarded
- JSON formatting
- The host directory is declared at container run-time

---

```Dockerfile
WORKDIR /path/to/workdir
```

Sets the working directory for any `RUN`, `CMD`, `ENTRYPOINT`, `COPY` and `ADD` instructions.
It can be used multiple times, and will be relative to the previous `WORKDIR` instruction.

```Dockerfile
WORKDIR /a  # /a
WORKDIR b   # /a/b
WORKDIR c   # /a/b/c
```

---

## List of things to be covered

- [ ] [The ENV file][the-env-file] - for securing sensitive info when running containers
- [ ] [Security][security-link]

[arg-from-interaction]: https://docs.docker.com/engine/reference/builder/#understand-how-arg-and-from-interact
[the-env-file]: https://docs.docker.com/compose/environment-variables/#/the-env-file
[security-link]: https://github.com/konstruktoid/Docker/blob/master/Security/CheatSheet.adoc
