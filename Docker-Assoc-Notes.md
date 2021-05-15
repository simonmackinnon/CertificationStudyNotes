# Certified Docker Associate

## Basic Information
- Can indentify a container by UUID, short ID, or name
- Can run containers in attached or detatched mode
    - advantage of detatched is retaining terminal control, no issues if command is interupted
- Stop all containers with command `docker container stop $(docker container ls -aq)`
- Remove all containers with command `docker container rm $(docker container ls -aq)`
- Many CLI commands that exist in the base (e.g. docker attach) are being moved to be child commands of docker container COMMAND (e.g. docker container attach)
    - This is the prefered approach going forward
- `docker container exec` runs a command in a running container
    - e.g. `docker container exec bash` or `docker container exec sh` allows access a bash terminal on the container
    - e.g. `docker container exec netstat -ntlp` will execute and return output from container to host terminal
    - processes started this way will exit if the primary container process (PID 1) exits (this triggers stop of container)
        - will not automatically restart if container is restarted
    - commands not available will result in an error, i.e. "executable file not found in $PATH".
        - add new application directories to PATH or add executable to /bin directory to enable use 
    - `-i` flag keeps stdin open
    - `-t` flag allocates a pseudo-TTY
- default container command 
    - executes with PID 1
    - is defined with the CMD directive in the dockerfile
    - override by:
        - altering dockerfile and create new image to run container from
        - alter at runtime i.e. `docker container run -d nginx sleep 500`
- restart policies
    - docker containers don't automatically start on host daemon restart
    - docker has restart policies which allows this to be controlled
        `--restart` flag controls this
- disk usage metrics
    - run `docker system df` to get docker level disk usage
    - run `docker system df -v` to get per component (images/repos/containers) level metrics
- use `--rm` flag to automatically delete containers when it exits, i.e. `docker container run -dt --rm --name testcontainer01 busybox ping -c10 google.com`
- general container info: `docker inspect <container name>`, i.e. get IP, etc.
 
## Image Creation, Management & Registry
- Image creation
    - Instructions for docker images contained in a file called Dockerfile
    - Images are built using `docker build .` 
    - The new image can be viewd in the `docker images` output
- Dockerfile
    - text file that has series of Instructions and arguments followed by the daemon to build a new image
    - Starts with the `FROM` instruction which is the base image the new one should be based on - required
    - Comments have a `#` at the start of the line
    - Simple use case:
        ```
        # base image is the ubuntu image
        FROM ubuntu

        # run/install the OS updates
        RUN apt-get update

        # install nginx package silently
        RUN apt-get install -y nginx

        # copy the host file to the image location
        COPY index.nginx-debian.html /var/www/html/

        # specify the command to be run as the default 
        #   process when the container starts
        CMD nginx -g 'daemon off;'
        ```
    - COPY vs. ADD commands
        - Serve similar purposes - both can copy a file to the image
        - COPY has `src` and `destination`
            - source is file on host directory
        - ADD also supports 2 other sources
            - URL (doesn't need to be local)
                - recommend using wget or curl for internet sourced files, multiple instructions create additional image layers
            - Extract compressed file to the destination
                `ADD compress.tar.gz /tmp` - performs decompression then moves them to the container
    - EXPOSE instruction
        - informs user about what port a specific app/service is running on the container which `-p` value needs to be set up
        - doesn't actually publish the port
        - example of `EXPOSE 9324` means the service will be running on port 9324, host would need to bind to this, e.g. `-p 8001:9324`
    - HEALTHCHECK instruction
        - how to test that the application is healthy
        - if the healthcheck command returns non-zero exit code, status is unhealthy,
        - if exit code is zero, status is healthy
        - before healthcheck has run, status is pending
        - specify `--interval` (default 30s), `--timeout` (default 30s), `--start-period` (default 0s)  and number of `--retries` (default 3)
    - ENTRYPOINT instruction
        - set the image's main command
        - doesn't allow you to override the command
            - can append arguments or further commands to the entry point command
    - Tagging Docker Images
        - tag images during build with '-t' command switch `docker build -t demo:v1 .`
        - tag untagged images post build wit 'tag' command `docker tag 30fba3457cd3a demo:v2`
        - tag existing tagged images with 'tag' command `docker tag ubuntu:latest myubuntu:v1`
    - Docker Commit
        - commit changes to a running container back to a (new) image
        - `docker container commit CONTAINER-ID myimage01`
        - `--change` option allows Dockerfile instruction changes to the image that gets created
            - CMD, ENTRYPOINT, ENV, EXPOSE, LABEL, ONBUILD, USER, VOLUME, WORKDIR
        - container and processes are paused while the new image is being committed
    - Layers of Docker Image
        - Docker images are made from a series of layers
        - Layers represent Dockerfile instructions
        ![Docker Layers](https://blog.florianlopes.io/content/images/2016/04/Docker-layers.png)


## Networking
- By default, Docker containers can make egress connections, but ingress connections don't work.
- To enable inbourn connections, need to bind the container to a host port

## Orchestration

## Installation & Configuration of Docker EE

## Security

## Storage & Volumes

## Exam Preparation
- udemy.com/course/docker-certified-associate