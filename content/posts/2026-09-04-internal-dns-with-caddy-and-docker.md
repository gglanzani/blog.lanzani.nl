---
title: Real HTTPS Certificates for Local Docker Services with Caddy and Let's Encrypt
author: Giovanni Lanzani
date: 2026-09-04T00:00:00Z
url: /2026/internal-dns-with-caddy-and-docker-containers
tags:
   - docker
   - DNS
   - wiki
---

At home, I run a number of docker containers on my Beelink S12[^1]. To easily access them with a memorable name, instead of [http://192.168.1.2:823](http://192.168.1.2:823), I was running [caddy-docker-proxy]. It proxies docker containers with caddy through labels in a `docker-compose.yml` file. With it, I could access each container at https://my-service.lan.

The basic usage is quite simple. First of all, we need caddy-docker-proxy running (once per server, so you'll serve multiple services running on their docker containers).

A sufficient `docker-compose.yml` looks like this:  

```yaml
services:
  caddy:
    image: caddy-docker-proxy
    ports:
      - 80:80
      - 443:443
    environment:
      - CADDY_INGRESS_NETWORKS=caddy
    networks:
      - caddy
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - caddy_data:/data
    restart: unless-stopped

networks:
  caddy:
    external: true

volumes:
  caddy_data: {}
```

Then, to run f.e. [adguard], I would use this `docker-compose.yml`:

```yaml
services:
  adguardhome:
    container_name: adguardhome
    image: adguard/adguardhome
    restart: unless-stopped
    volumes:
      - $DATA/adguard/work:/opt/adguardhome/work
      - $DATA/adguard/conf:/opt/adguardhome/conf
    user: 1000:1000
    networks:
      - caddy
    labels:
      caddy: adguard.lan
      caddy.reverse_proxy: "{{upstreams 80}}"
    ports:
      - "53:53/tcp"
      - "53:53/udp"

networks:
  caddy:
    external: true
```

This is all you need to access the service through https://adguard.lan.

However, you need to trust local certificates (my [post] about it is the most popular of my blog) if you don't want to be bothered by safety warnings. Every. Single. Time.

Some time ago, however, I read that's possible to issue real HTTPS certificates with [Let's Encrypt](https://letsencrypt.org/) even if the docker container is not reachable from for the internet.

How?

The first step is to see whether [caddy-dns] supports your DNS registrar. Then, you need to create an API key and build a custom version of caddy-docker-proxy (in the example below, I've used [porkbun] but YMMV):

```Dockerfile
ARG CADDY_VERSION=2.11
FROM caddy:${CADDY_VERSION}-builder AS builder

RUN xcaddy build \
    --with github.com/lucaslorentz/caddy-docker-proxy/v2 \
    --with github.com/caddy-dns/porkbun

FROM caddy:${CADDY_VERSION}-alpine

COPY --from=builder /usr/bin/caddy /usr/bin/caddy

CMD ["caddy", "docker-proxy"]
```

(build this image with `docker build -t caddy-caddy-docker-proxy-porkbun .`)

Afterward, you can edit the previous compose files (note the changed image name and `labels` section):

```yaml
services:
  caddy:
    image: caddy-docker-proxy-porkbun
    ports:
      - 80:80
      - 443:443
    environment:
      - CADDY_INGRESS_NETWORKS=caddy
    networks:
      - caddy
    labels:
      caddy.acme_dns: porkbun
      caddy.acme_dns.api_key: "$PORKBUN_API_KEY"
      caddy.acme_dns.api_secret_key: "$PORKBUN_API_SECRET_KEY"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - caddy_data:/data
    restart: unless-stopped

networks:
  caddy:
    external: true

volumes:
  caddy_data: {}
```

For adguard, update the `labels.caddy` key with the desired subdomain (and don't forget to point your subdomain in your DNS registrar to the **internal** ip address where caddy-docker-proxy is running! In my case, that's an A record pointing to 192.168.1.2)

```yaml
version: '3.3'
services:
  adguardhome:
    container_name: adguardhome
    image: adguard/adguardhome
    restart: unless-stopped
    volumes:
      - $DATA/adguard/work:/opt/adguardhome/work
      - $DATA/adguard/conf:/opt/adguardhome/conf
    user: 1000:1000
    networks:
      - caddy
    labels:
      caddy: adguard.lanzani.nl
      caddy.reverse_proxy: "{{upstreams 80}}"
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "81:80/tcp"
      - "3000:3000/tcp"

networks:
  caddy:
    external: true
```

And that's it! Now https://adguard.lanzani.nl will be served without security warnings (while still being *inaccessible* from outside your network, if you set it up like me).

[caddy-docker-proxy]: https://github.com/lucaslorentz/caddy-docker-proxy
[post]: /2024/trust-local-caddy-certificates-on-macos/
[caddy-dns]: https://github.com/orgs/caddy-dns/repositories?language=&q=&sort=&type=all
[porkbun]: https://porkbun.com/
[adguard]: https://adguard.com/

[^1]: What a steal if I look at the May 2025 pricing (when I bought it) versus today.
