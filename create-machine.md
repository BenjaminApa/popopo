# docker machine

example script

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
    --digitalocean-size "2gb" \
    $1
```

```bash
eval $(docker-machine env <machine-name>)
```