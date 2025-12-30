# WARNING

See [here](../basic/README.md) for the log file pitfall 

# Description

This example explains how to integrate Crowdsec in environment deployed with docker-compose. It set up multiple containers :

This example contains multiple containers :
* app : apache server serving index.html containing an `hello world`
* openresty : openresty built on nginx that serving this app from the host
* crowdsec : it will read reverse-proxy logs from the shared volume
* dashboard : we use [metabase](https://hub.docker.com/r/metabase/metabase) to display crowdsec database data.

## Openresty

The example builds om the https://registry.hub.docker.com/r/crowdsecurity/openresty docker image, as it comes with the bouncer plugin bundled.
The configuration is passed by by the enviroment variable `BOUNCER_CONFIG`. The referenced secrect e.g. `${CROWDSEC_BOUNCER_OPENRESTY_APIKEY}` are passed via a `.env` file. 

The `APPSEC_URL` feature needs is activated in the `crowdsec` container itself.

For website serving the file [default.conf](openresty/conf.d/default.conf) shows a plain port 80 http example and a SSL https example. Note the [certificate](https://nginx.org/en/docs/http/configuring_https_servers.html) and key need to be provided, to run this.

## Crowdsec

Note the `BOUNCER_KEY_openresty` enviroment variable. It regesiters the Bouncer with an own key. Yes, that means the `${CROWDSEC_BOUNCER_OPENRESTY_APIKEY}` value can be choosen in the `.env` file. Recommendation is to use a random UUID.

`APPSEC` in configured by adding the collections  `crowdsecurity/appsec-virtual-patching` and `crowdsecurity/appsec-generic-rules`, as well as in [acquis.yaml](crowdsec/acquis.yaml). Note [link](https://docs.crowdsec.net/u/getting_started/post_installation/acquisition_new/) shows how to cleanly do this.

# Prerequisites:
[Docker](https://docs.docker.com/engine/install/) / [Docker Compose](https://docs.docker.com/compose/install/)

# Discussions

[GitHub --> Networking - userland-proxy could better clarify impact #17312](https://github.com/docker/docs/issues/17312)

# ps: default's credentials for metabase are `crowdsec@crowdsec.net` and `!!Cr0wdS3c_M3t4b4s3??`
