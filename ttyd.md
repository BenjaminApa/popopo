### ttyd

https://github.com/tsl0922/ttyd

```bash
docker run \
    -it \
    --rm \
    -p 7682:7681 \
    -v /var/run/docker.sock:/var/run/docker.sock:ro \
    -v /usr/bin/docker:/usr/bin/docker \
    -v /polytech:/polytech \
    -v /.docker/:/root/.docker \
    --entrypoint="" \
    tsl0922/ttyd ttyd -c polytech:tuningforever bash
```

### misc setup notes

```bash
wget https://github.com/docker/machine/releases/download/v0.14.0/docker-machine-Linux-x86_64
chmod +x docker-machine-Linux-x86_64
alias dm='/docker-machine-Linux-x86_64'
open http://$(docker-machine ip test1):7682
apt-get update && apt-get install -y libltdl7
```

# ttyd + Alpine Linux

```bash
docker run \
    -it \
    --rm \
    -p 7682:7681 \
    -v /var/run/docker.sock:/var/run/docker.sock:ro \
    --entrypoint="" \
    tsl0922/ttyd:alpine ttyd -c polytech:tuningforever bash
```

https://alpinelinux.org/
https://pkgs.alpinelinux.org/packages?name=docker&branch=edge
https://wiki.alpinelinux.org/wiki/Alpine_Linux_package_management

```shell
echo @edge http://nl.alpinelinux.org/alpine/edge/main >> /etc/apk/repositories
echo @edge http://nl.alpinelinux.org/alpine/edge/community >> /etc/apk/repositories
cat /etc/apk/repositories
apk update  --update-cache --allow-untrusted
apk add docker@edge
apk add wget
wget https://github.com/docker/machine/releases/download/v0.14.0/docker-machine-Linux-x86_64
```
