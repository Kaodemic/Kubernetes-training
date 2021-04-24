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