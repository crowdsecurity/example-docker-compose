# Docker Compose

This example explains how to integrate Crowdsec in environment deployed with docker-compose. It set up multiple containers :

![Schema](schema.png)

This example contains multiple containers :
* app : apache server serving index.html containing an `hello world`
* reverse-proxy : nginx that serving this app from the host
* crowdsec : it will read reverse-proxy logs from the shared volume
* dashboard : we use [metabase](https://hub.docker.com/r/metabase/metabase) to display crowdsec database data.

We have chosen the simplest way to collect logs (by sharing volumes between containers), if you are in production, you are probably using [logging-driver](https://docs.docker.com/config/containers/logging/configure/) to centralize logs with rsyslog or another driver, so don't forget to adapt the crowdsec docker-compose configuration to read your logs properly.

**Prerequisites:** [Docker](https://docs.docker.com/engine/install/) / [Docker Compose](https://docs.docker.com/compose/install/)

ps: default's credentials for metabase are `crowdsec@crowdsec.net` and `!!Cr0wdS3c_M3t4b4s3??`
