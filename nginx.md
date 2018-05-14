# Nginx Samples

```nginx.conf

# basic setup
user  nginx;
worker_processes  1;
error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;
events { worker_connections  1024; }

http {

    # Rewrite HTTP requests to HTTPS
    server {
        listen         80;
        listen    [::]:80;
        return         301 https://$host$request_uri;
    }

    # add ssl
    ssl_certificate     /etc/letsencrypt/live/polytech/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/polytech/privkey.pem;


    # load-balance several 'api' services
    server {
        listen 443 ssl;
        listen [::]:443 ssl;
        server_name api.decihub.com;
        location /ws {
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            proxy_http_version 1.1;

            proxy_pass http://api:8080;
        }

        location / {
            include transparent.conf;
            proxy_pass http://api:8080;
        }
    }

}
```


--------

### sample dockerfile

```dockerfile
# https://hub.docker.com/_/nginx/
FROM nginx:alpine

EXPOSE 80
EXPOSE 443
ADD src/transparent.conf /etc/nginx/transparent.conf
ADD src/.htpasswd /etc/nginx/.htpasswd
ADD src/nginx.conf /etc/nginx/nginx.conf

# check that the configuration is correct
# -> can't check at build time since upstream do not exist yet
# RUN nginx -t -c /etc/nginx/nginx.conf
```

--------

### example debug commands

```shell
# run a container with tty attached
docker run -it -v /.../nginx.conf:/etc/nginx/nginx.conf --rm my-nginx-image bash 

# check nginx syntax
nginx -t -c /etc/nginx/nginx.conf
```


----------

### doc snippets

random nginx doc and snippets used to craft above config. 

- http://nginx.org/en/docs/http/configuring_https_servers.html
- http://piotrnowicki.com/java/2017/01/09/keycloak-docker-with-ssl-proxy/
- https://raw.githubusercontent.com/best-coloc-ever/twitch-cast/b01ea69ac3431063525a6d73d78b2d6063ed99a6/frontend/server/nginx.conf
- https://serverfault.com/questions/67316/in-nginx-how-can-i-rewrite-all-http-requests-to-https-while-maintaining-sub-dom
- https://stackoverflow.com/questions/29466494/nginx-turn-off-cache-for-a-specific-file
- https://support.cloudflare.com/hc/en-us/articles/214534978-Are-the-HTTP-2-or-SPDY-protocols-supported-between-Cloudflare-and-the-origin-server-
- https://www.garron.me/en/linux/nginx-reverse-proxy.html
- https://www.nginx.com/blog/websocket-nginx/
- https://www.nginx.com/resources/wiki/start/topics/tutorials/config_pitfalls/#taxing-rewrites
- https://www.tutorialspoint.com/articles/how-to-configure-nginx-as-reverse-proxy-for-websocket
