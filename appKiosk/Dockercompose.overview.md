# Docker Compose 
Docker Compose is the very tool that enables us to run multiple containers with ease, and it's a built-in tool in the Docker CE distribution. All it does is read `dockercompose.yml `(or .yaml) to run defined containers. A `docker-compose` file is a YAML based template, and it typically looks like this:

```
version: '3'
services:
hello-world:
image: hello-world
```
Launching it is pretty simple: save the template to `docker-compose.yml` and use the
`docker-compose up `command to start it:

```
$ docker-compose up
Creating network "cwd_default" with the default driver
Creating cwd_hello-world_1
Attaching to cwd_hello-world_1
hello-world_1 |
hello-world_1 | Hello from Docker!
hello-world_1 | This message shows that your installation appears to be working correctly.
...
cwd_hello-world_1 exited with code 0
```
Let's see what `docker-compose` did behind the `up` command

The `docker-compose.yml `file consists of configurations of `volumes`, `networks`, and
`services`