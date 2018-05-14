# Let's encrytp


1. create a new 'ssl' folder and cd into it
2. create a dockerfile based on `certbot/certbot` if needed
3. craft a command to get a certificate `--standalone`
4. ask me to setup a dns entry so your machine is publicly available.
5. get a certificate
6. package https demo app in `package.md`, and run it so It's accessible on port 443

----------

Below are some examples from my projects


```dockerfile
FROM certbot/certbot:v0.23.0
RUN apk add --update curl bash docker
RUN pip install certbot-dns-cloudflare
RUN pip install certbot-dns-google
ADD scripts /scripts
WORKDIR /scripts
ENTRYPOINT []
```

```shell
#! /bin/sh
set -eu

certbot certonly \
    --work-dir=/letsencrypt/lib \
    --logs-dir=/letsencrypt/logs \
    --config-dir=/letsencrypt/etc \
    --non-interactive \
    --keep-until-expiring \
    --dns-cloudflare \
    --dns-cloudflare-credentials=/run/secrets/credentials.ini \
    --email=vion.remi@gmail.com \
    --no-eff-email \
    --agree-tos \
    --cert-name=decihub \
    -d app.decihub.com,api.decihub.com,auth.decihub.com,www.decihub.com
```

```shell
#! /bin/bash
set -eu
docker service create \
    --replicas 1 \
    --with-registry-auth \
    --name certbot \
    --constraint 'node.labels.web == true' \
    --restart-condition none \
    --mount type=bind,source=/letsencrypt,destination=/letsencrypt \
    --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock \
    --secret source=dns_key,target=credentials.ini \
    $IMAGE /scripts/update-swarm-certs.sh
```

```shell
#! /bin/bash
set -u

docker node ls -q | xargs docker node inspect \
  -f '{{ .ID }} [{{ .Description.Hostname }}]: {{ range $k, $v := .Spec.Labels }}{{ $k }}={{ $v }} {{end}}'
```
