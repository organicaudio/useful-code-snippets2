# docker setup postgres

## postgres docker image

[Documentation for postgress image](https://hub.docker.com/_/postgres?tab=description)
[Nice tutorial](https://info.crunchydata.com/blog/easy-postgresql-10-and-pgadmin-4-setup-with-docker)

```shell
# create a user-defined bridge network so containers in it can address themself by the container name not only by ip address
docker network create --driver bridge pgnetwork

# create volume so db will be saved
docker volume create --driver local --name=pgvolume

# create docker container for postgres
docker run -d --rm \
--name pg-container \
-p 5432:5432 \
-e POSTGRES_USER=postgres \
-e POSTGRES_PASSWORD=postgres \
--network=pgnetwork \
--volume=pgvolume:/var/lib/postgresql/data \
postgres:latest


# standard db name is postgres. you can set it via:
# -e POSTGRES_DB=postgres 
```
**NOTE**: if you restart this container with new passwords you may need to clear the volume. Inside of the container you can use the psql to interact with the database.


## pgAdmin docker image

[Documentation for pgAdmin image](https://www.pgadmin.org/docs/pgadmin4/latest/container_deployment.html)

```shell
docker run -d --rm \
--name pgAdmin-container \
-p 9080:80 \
-e PGADMIN_DEFAULT_EMAIL=example@mail.com \
-e PGADMIN_DEFAULT_PASSWORD=limoneneis \
--network=pgnetwork \
dpage/pgadmin4:4.20
```

1. After starting the container
2. Open http://localhost:9080/login in browser
3. login with your PGADMIN_DEFAULT_EMAIL and your PGADMIN_DEFAULT_PASSWORD
4. add server with
   - host name address: pg-container
   - username: postgres
   - password: postgres


## known issue

1. An process named oka uses 100 of the cpu [see](https://dba.stackexchange.com/questions/44084/troubleshooting-high-cpu-usage-from-postgres-and-postmaster-services)
2. When an application tries to connect to postgres for the second time the authentification fails because of an invalid password. 