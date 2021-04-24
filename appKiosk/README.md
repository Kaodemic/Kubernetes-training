This is an application to demonstrate how components interact with each others.

*Kiosk* is a service with three APIs, and it connect to a *redis* to cache its tickets. Tickets are set to zero by default, and they can be set with `POST /tickets`:
```
$ curl -XPOST -F 'value=10' <kiosk-service>/tickets
```
Our kiosk now can start its business. Use `GET /tickets` to know how many tickets left, And use `POST /buy` to consume one ticket.

```
// Get tickets
$ curl -XGET <kiosk-service>/tickets
// Buy one ticket
$ curl -XPOST <kiosk-service>/buy
```

## What do you think when we run this bash
``` bash
$ docker network create kiosk
$ docker run -d -p 5000:5000 \
    -e REDIS_HOST=lcredis --network=kiosk kiosk-example
$ docker run -d --network-alias lcredis --network=kiosk redis
$ docker run -d -e REDIS_HOST=lcredis -e MYSQL_HOST=lmysql \
-e MYSQL_ROOT_PASSWORD=$MYPS -e MYSQL_USER=root \
--network=kiosk recorder-example
$ docker run -d --network-alias lmysql -e MYSQL_ROOT_PASSWORD=$MYPS \
--network=kiosk mysql:5.7
```
> Everything works well so far. However, if next time we want to launch the same stack again, our applications are very likely to start up prior to the databases, and
they might fail if any incoming connection requests any change against the databases. In other words, we have to consider the startup order in our startup scripts.
Additionally, scripts are also inept with problems such as how to deal with a random components crash, how to manage variables, how to scale out certain components,
and so on

## What are different between `docker run` vs `docker-compose run`
> Docker Compose is basically a medley of Docker functions for multiple containers.
For example, the counterpart of docker build is docker-compose build; the previous one builds a Docker image, and so the later one builds Docker images listed in the
docker-compose.yml. But there's one thing that needs to be pointed out: the dockercompose run command is not the correspondent of docker run; it's running a specific container from the configuration in the docker-compose.yml. In fact, the closest command to docker run is docker-compose up.

## What are network default when we use `docker-compose up`?
> Since there is no network defined, docker-compose would create a new network with a default driver and connect services to the same network.
Additionally, the network name of a container would be the service's name. You may
notice that the name displayed in the console slightly differs from its original one in the docker-compose.yml. It's because Docker Compose tries to avoid name conflicts between containers. As a result, Docker Compose runs the container with the name it generated, and makes a network-alias with the service name.