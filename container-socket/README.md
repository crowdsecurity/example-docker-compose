## Description

This example show how you can use the container socket to read logs from other containers without mounting volumes.

![Schema](schema.png)

This example contains multiple containers :
* app : Tiny Go webserver that prints OS information and HTTP request to output.
* reverse-proxy : nginx that serving this app from the host
* crowdsec : it will read reverse-proxy logs from the socket
* socket-proxy: HAProxy that will expose container socket over TCP instead of mounting the socket directly into CrowdSec container

**Prerequisites:** [Docker](https://docs.docker.com/engine/install/) / [Docker Compose](https://docs.docker.com/compose/install/)
