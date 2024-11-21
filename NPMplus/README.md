## Description

This example shows you [NPMplus](https://github.com/ZoeyVid/NPMplus).

This example contains multiple containers:
* crowdsec : Read NPMplus logs from the mounted volumes
* NPMplus : The NPMplus container

You also need to create a secure api key for the bouncer in crowdsec and configure NPMplus

```bash
sed -i 's|"TZ=.*"|"TZ=<your-timezone>"|g' compose.yaml # please set your timezone here, an example would be Europe/Berlin
sed -i 's|"ACME_EMAIL=.*"|"ACME_EMAIL=<your-email>"|g' compose.yaml # please set an email here
docker compose up -d
docker exec crowdsec cscli bouncers add npmplus -o raw
sed -i "s|API_KEY=.*|API_KEY=<key shown from above output>|g" /opt/npm/etc/crowdsec/crowdsec.conf # please set the api key here
sed -i "s|ENABLED=.*|ENABLED=true|g" /opt/npm/etc/crowdsec/crowdsec.conf
docker restart npmplus
```

**Prerequisites:** [Docker](https://docs.docker.com/engine/install) / [Docker Compose](https://docs.docker.com/compose/install)
