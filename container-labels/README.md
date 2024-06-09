## Description

This example show how you can use the container labels to discover the containers you want to protect.

This example contains multiple containers :
* app : Tiny Go webserver that prints OS information and HTTP request to output.
* reverse-proxy : nginx that serving this app from the host
* reverse-proxy2 : nginx that serving this app from the host
* crowdsec : it will read reverse-proxy logs from the socket
* socket-proxy: Nginx container that will expose container socket over TCP instead of mounting the socket directly into CrowdSec container

Attribution to [linux server](https://www.linuxserver.io/) team for the docker-socket-proxy container.

**Prerequisites:** [Docker](https://docs.docker.com/engine/install/) / [Docker Compose](https://docs.docker.com/compose/install/)

## Docker socket mounted to Socket-Proxy

![docker-no-socket](docker-socket-proxy.png)

Example above shows the docker socket mounted to the Socket-Proxy container. This is best practice and should be used instead of mounting the socket directly into the CrowdSec container.

## How does this setup work?

CrowdSec container will request from the socket-proxy the running container information. If the running containers contains the label `crowdsec=true` and `crowdsec.labels.type=<type>` it will be added to the CrowdSec configuration without having to specify the container name or regex. This is useful when running in a dynamic environment where containers are created and destroyed frequently.
