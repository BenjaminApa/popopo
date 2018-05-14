### ttyd

https://github.com/tsl0922/ttyd

```bash
docker run \
 -it \
 --rm \
 -p 7682:7681 \
 -v /var/run/docker.sock:/var/run/docker.sock:ro \
 --entrypoint="" \
 tsl0922/ttyd ttyd -c polytech:tuningforever
```

# ttyd + Alpine Linux

```bash
docker run \
    -it \
    --rm \
    -p 7682:7681 \
    -v /var/run/docker.sock:/var/run/docker.sock:ro  \
    --entrypoint="" \
    tsl0922/ttyd:alpine ttyd -c polytech:tuningforever
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
