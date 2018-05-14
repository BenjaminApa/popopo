# docker compose

1. create a docker compose file for your webserver (without nginx)
2. try to deploy a working nginx load balancer (you need to craft a nginx image first)


# node labels

1. add

```sh
docker node update --label-add db=true m2
```

2. check

```shell
#! /bin/bash
set -u
docker node ls -q | xargs docker node inspect \
  -f '{{ .ID }} [{{ .Description.Hostname }}]: {{ range $k, $v := .Spec.Labels }}{{ $k }}={{ $v }} {{end}}'
```

# compose file

```yaml
version: "3.3"

services:
    nginx:
        image: ${DECIHUB_IMAGE_NGINX:-ERROR}
        ports:
            - { target: 80, published: 80, protocol: tcp, mode: host }
            - { target: 443, published: 443, protocol: tcp, mode: host }
        networks: [ "backend" ]
        volumes: [ "certs:/etc/letsencrypt" ]
        deploy:
            replicas: 1
            placement: { constraints: [ node.labels.web == true ] }

    api:
        image: ${POLYTECH_IMAGE_API:-ERROR}
        networks: [ "backend" ]
        deploy: { replicas: 3 }

networks:
    backend:

volumes:
    certs:
        external: true

```

------------

random websites used while crafting this compose file
- https://forums.docker.com/t/only-the-first-service-port-is-getting-exposed-when-doing-a-stack-deploy-in-docker-swarm/27642
- https://forums.docker.com/t/docker-swarm-published-port-limilation/35923/4
- https://docs.docker.com/compose/compose-file/#volumes
