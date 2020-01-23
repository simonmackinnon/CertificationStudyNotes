# Certified Docker Associate

## Basic Information
- Can indentify a container my UUID, short ID, or name
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

## Image Creation, Management & Registry

## Networking
- By default, Docker containers can make egress connections, but ingress connections don't work.
- To enable inbourn connections, need to bind the container to a host port

## Orchestration

## Installation & Configuration of Docker EE

## Security

## Storage & Volumes

## Exam Preparation
- udemy.com/course/docker-certified-associate