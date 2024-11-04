## Description

This example shows you a [NPM](https://nginxproxymanager.com/) example using LePresidente FORK.

This example contains multiple containers :
* crowdsec : Read NPM logs from the mounted volumes
* npm : The Nginx Proxy Manager container

Before running the example, you need to create a secure database password within the `.env` file. You can do this by running the following command :

```bash
echo "ROOT_DATABASE_PASSWORD=$(tr -dc A-Za-z0-9 </dev/urandom | head -c 32)" > .env
echo "DATABASE_PASSWORD=$(tr -dc A-Za-z0-9 </dev/urandom | head -c 32)" >> .env
```

You also need to create a secure api key for the bouncer in crowdsec

```bash
docker compose up crowdsec -d
docker compose exec crowdsec cscli bouncer add npm-bouncer
echo "CROWDSEC_BOUNCER_APIKEY=<key shown from above output>" >> .env
docker compose down
```

By default, NPM does not report the original client's IP, but rather the CDN or other proxy IP in between the client and your server. If you run Crowdsec, it will ban this middle-man IP rather than the source IP. 
In order to ban the source IP, use the file in `/data/nginx/custom/server_proxy.conf`. [View here for more information](https://nginxproxymanager.com/advanced-config/#custom-nginx-configurations). This will set `real_ip_header X-Forwarded-For` to all proxy hosts globally. 

Then you can start the containers as normal 

```bash
docker compose up -d
```

**Prerequisites:** [Docker](https://docs.docker.com/engine/install/) / [Docker Compose](https://docs.docker.com/compose/install/)
