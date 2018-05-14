# Setup Swarm cluster

1.  Init first swarm master

```sh
$ eval `dm env mdh1`
$ docker swarm init --advertise-addr 10.135.15.71
Swarm initialized: current node (304cpbn29q0849hk5syhej76l) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-24y3efkrdxc0hwqnx4fkn3pbx0vbj2bz2yhv7mqpg81zemjuiu-b1keqhhw62ct3pst0wohevfva 10.135.15.71:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```

2.  Retrieve "join swarm as manager" command
    (because the command above is to join the swarm as slave)

```sh
$ docker swarm join-token manager
To add a manager to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-24y3efkrdxc0hwqnx4fkn3pbx0vbj2bz2yhv7mqpg81zemjuiu-6m7udatyekw0c20qvo9mvxahy 10.135.15.71:2377
```

3.  join 2rd host

```sh
$ eval `dm env mdh2`
$ docker swarm join --token SWMTKN-1-24y3efkrdxc0hwqnx4fkn3pbx0vbj2bz2yhv7mqpg81zemjuiu-6m7udatyekw0c20qvo9mvxahy 10.135.15.71:2377
This node joined a swarm as a manager.
```

4.  join 3nd host

```sh
$ eval `dm env mdh3`
$ docker swarm join --token SWMTKN-1-24y3efkrdxc0hwqnx4fkn3pbx0vbj2bz2yhv7mqpg81zemjuiu-6m7udatyekw0c20qvo9mvxahy 10.135.15.71:2377
This node joined a swarm as a manager.
```

5.  check cluster state:

```sh
$ dm ls
NAME   ACTIVE   DRIVER         STATE     URL                         SWARM   DOCKER        ERRORS
mdh1   -        digitalocean   Running   tcp://46.101.100.163:2376           v18.03.0-ce
mdh2   -        digitalocean   Running   tcp://159.89.8.99:2376              v18.03.0-ce
mdh3   *        digitalocean   Running   tcp://159.65.126.128:2376           v18.03.0-ce
```

```sh
$ docker node update --label-add web=true 304cpbn29q0849hk5syhej76l
$ generator/cluster/swarm/node-labels.sh
304cpbn29q0849hk5syhej76l [mdh1]: web=true
zezy4e4fmn1duysi93xot7v16 [mdh2]:
n70uyfcf8c84ntlj3f36xd3fs [mdh3]:
```

---

# DB

```sh
docker run -it --rm postgres:alpine sh
psql postgres://avnadmin....
# copy db initialization script (see init-database.sh)
```

```sh
echo $PG_HOST | docker secret create pg_host -
echo $PG_PORT | docker secret create pg_port -
echo $PG_PASSWORD_DECIHUB | docker secret create pg_password_decihub -
echo $PG_PASSWORD_KEYCLOAK | docker secret create pg_password_keycloak -
cat privkey.pem | docker secret create ssl_privkey -
cat fullchain.pem | docker secret create ssl_fullchain -
```
