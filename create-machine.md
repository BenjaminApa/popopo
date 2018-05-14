# docker machine

buy machine :

```bash
#! /bin/bash

set -u # ensure a machine name has been properly given
echo machine name : $1

# https://docs.docker.com/machine/get-started-cloud/
# https://docs.docker.com/machine/drivers/digital-ocean/
docker-machine version

docker-machine create \
    --driver digitalocean \
    --digitalocean-access-token $POLYTECH_DIGITALOCEAN_TOKEN \
    --digitalocean-private-networking \
    --digitalocean-image="ubuntu-16-04-x64" \
    --digitalocean-region="fra1" \
    --digitalocean-size "1gb" \
    $1
```

```bash
eval $(docker-machine env <machine-name>)
```

----------

add commnent above each command to describe what it does.

```shell
# 
docker-machine env test1

# 
eval $(docker-machine env test1)

#
docker-machine ip test1

#
ls -la $HOME/.docker/machine

#
ls -la $HOME/.docker/machine/machines/test1

#
docker-machine ssh test1

#
ssh -i $HOME/.docker/machine/machines/test1/id_rsa $(docker-machine ip test1)
```
