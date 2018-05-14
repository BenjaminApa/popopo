use following script to get list of available options for docker-machine digitalocean driver:

```shell
#! /bin/bash

curl \
    -X GET \
    --silent \
    "https://api.digitalocean.com/v2/images?per_page=999" \
    -H "Authorization: Bearer $DECIHUB_DIGITALOCEAN_TOKEN" \
    | jq '.' > do-images.json

curl \
    -X GET \
    --silent \
    "https://api.digitalocean.com/v2/regions?per_page=999" \
    -H "Authorization: Bearer $DECIHUB_DIGITALOCEAN_TOKEN" \
    | jq '.' > do-regions.json

curl \
    -X GET \
    --silent \
    "https://api.digitalocean.com/v2/sizes?per_page=999" \
    -H "Authorization: Bearer $DECIHUB_DIGITALOCEAN_TOKEN" \
    | jq '.' > do-sizes.json
```
