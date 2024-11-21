## Description

This example shows you [NPMplus](https://github.com/ZoeyVid/NPMplus).

This example contains multiple containers:
* crowdsec : Read NPMplus logs from the mounted volumes
* NPMplus : The NPMplus container

You also need to create a secure api key for the bouncer in crowdsec

```bash
docker compose up -d
docker exec crowdsec cscli bouncers add npmplus -o raw
sed -i "s|API_KEY.*|API_KEY=<key shown from above output>"|g" /opt/npm/etc/crowdsec/crowdsec.conf
sed -i "s|ENABLED.*|ENABLED=true|g" /opt/npm/etc/crowdsec/crowdsec.conf
docker restart npmplus
```

**Prerequisites:** [Docker](https://docs.docker.com/engine/install) / [Docker Compose](https://docs.docker.com/compose/install)
