## Description

This example explains how to integrate Crowdsec in environment deployed with docker-compose. It set up multiple containers :

![Schema](schema.png)

This example contains multiple containers :
* app : Tiny Go webserver that prints OS information and HTTP request to output.
* reverse-proxy : nginx that serving this app from the host
* crowdsec : it will read reverse-proxy logs from the socket

**Prerequisites:** [Docker](https://docs.docker.com/engine/install/) / [Docker Compose](https://docs.docker.com/compose/install/)

ps: default's credentials for metabase are `crowdsec@crowdsec.net` and `!!Cr0wdS3c_M3t4b4s3??`
