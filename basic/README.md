## WARNING

This example still works, however, there is one pitfall that is not properly explained in this example.

When using the default nginx image the logs are written to the default log path `/var/log/nginx/access.log`, however, the nginx image symlinks these files to `/dev/stdout` so when you do `docker logs nginx` it shows them.

### Why is this a pitfall?

In this example CrowdSec is configured to read logs from files, however, it cannot read logs written to the container stdout using the file module. Please see [container-socket](../container-socket/) for an example on how to read logs from the container stdout.

### Potential fixes

So there are couple of fixes to this issue, either:
- Use a custom nginx image that deletes the symlinked files
```
FROM nginx:latest
RUN rm /var/log/nginx/*.log
```
- In our example the logs are written to `logs` docker volume so you can also just delete the symlinked files in the volume and then restart the container
```
cd $(docker volume inspect logs | jq '[].Mountpoint')
rm *.log && cd -
docker compose restart nginx
```
- Change the default log path in the nginx config to something else, for example `/var/log/nginx/example_access.log`
```
    access_log /var/log/nginx/example.access.log;
    error_log /var/log/nginx/example.error.log;
```

In this docker compose example we use the [latter option](reverse-proxy/nginx.conf#L13-L14) and update our [acquisition config](crowdsec/acquis.yaml#L2) to read from the new path.

## Description

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
