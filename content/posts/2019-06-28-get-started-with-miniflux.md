---
date: 2019-06-28T00:00:00Z
title: Get started with miniflux
tags: ["docker", "gcp", "miniflux", "programming", "rss", "ruby", "technology review"]
url: /2019/miniflux
---

This is another post that is totally a note for my future self.

I don't write on this blog often. But what I do, a lot, is read what other people write on their
blog. I do that through the wonderful capabilities of [RSS].

Doing so in a sane manner involves a few moving parts:

- One or multiple feeds you want to read. This is the easy part;
- A server that keeps track of them
- The server should provide a good interface to read on the web;
- Bonus points if the server also provides an API so that I can use apps to read the articles.

Up until a couple of weeks ago I was using a simple pair: [Stringer], hosted on a spare GCP
machine, and [Unread] on iOS. Stringer offers a nice reading experience on the web, so I didn't
need an app for my Mac.

However, as the spare machine wasn't spare anymore I started looking for something else as
I did not like the fact that Stringer was an unmaintained Ruby app anymore. I have nothing against
Ruby, but the fact that the app was unmaintained meant running a potentially insecure application.

There are many RSS readers as a service since Google Reader [shut down]:

- Feedbin
- Newsblur
- Bazqux
- The Old Reader
- And many more.

The only "problem" is that these services cost from approximately $2 to $5 a month. Can I do
something for free?

At first I thought about running stringer on one of my Raspberry Pis. They are pretty powerful and
I don't have that many feeds I need to read.

But if I do that, I possibly want to have everything working in a semi-automatic fashion, so that
there's little to no manual work if the SD is my Raspberry Pi goes [south].

The easiest solution — for single machine [scenarios] and where seconds of downtime are OK — is to
use [Docker] with [docker-compose].

This is where, however, Ruby and the Ruby version stringer uses (2.3.3) are painful:

- There were no official Ruby 2.3.3 images for ARM (that I could find);
- Updating to the latest 2.3 version (2.3.8 at the time of this writing) would trigger some bug
  that I was not able to fix;
- Updating to 2.3.4 would have everything working, but
- The image for stringer using Ruby 2.3.4 is 857MB: not exactly small;
- As I would need to customize the stringer docker image heavily (to make it compatible with 2.3.4,
  plus some other small details), that would mean building the images from time to time when new
  security updates get pushed (if they ever do);
- Doing the above is doable with one of the many CI/CD services out there (for example Azure
  [Pipelines]) but since the build needs to be for ARM on an X86 server, extra care and
  configuration is needed ([buildx] is not generally available yet).

If you're a bit like me, the above feels like a chore and change of many headaches (that's probably
why all those RSS as a service services exist in the first place).

So I turned to Reddit to see what others are doing. While searching here
and there, I came across a thread where they mention [miniflux].

When I looked at the website, I couldn't believe it: it has everything I need and then some more:

- Easy to get started with;
- Provides ARM images out of the box;
- Written in Go and hence very small to host (compared to the 857MB of stringer, miniflux docker
  image is 17MB, 50x smaller);
- Written in Go and hence faster than non-optimized Ruby (and it certainly feels a lot faster than stringer);
- Like stringer, it implements the [Fever] API, meaning I can use it with Unread.

## Requirements

Now that I have settled down on the server, what else do I need?

- I already said that I want Docker support;
- Possibly everything should be scriptable, for 99% of the code (I am OK running a couple of
  scripts manually);
- I want `https`: if I'm entering a password it should be secure;
- I (ideally) want the latest security patches quickly, without much maintenance;
- It should be easy.

## The solution

After a bit of googling, I've come up with the following folder structure and files to serve my
needs:

```
.
├── data
│   └── nginx
│       └── app.conf
├── docker-compose.yml
└── init-letsencrypt.sh
```

Let's see the content of each file.

{{< highlight yaml >}}
version: '3'
services:
  database.postgres:
    image: postgres:9.6-alpine
    container_name: postgres
    ports:
      - 5432:5432
    environment:
      - POSTGRES_PASSWORD=<insert_pg_password>
      - POSTGRES_USER=miniflux
      - POSTGRES_DB=miniflux
    volumes:
      - ./data/postgres:/var/lib/postgresql/data
    restart: always

  service.rss:
    image: miniflux/miniflux:latest
    container_name: miniflux
    ports:
      - 8080:8080
    depends_on:
      - database.postgres
    environment:
      - DATABASE_URL=postgres://miniflux:<insert_pg_password>@database.postgres:5432/miniflux?sslmode=disable
      - RUN_MIGRATIONS=1
      - CREATE_ADMIN=1
      - ADMIN_USERNAME=admin
      - ADMIN_PASSWORD=<insert_miniflux_password>
    restart: always

  nginx:
    image: nginx
    restart: unless-stopped
    volumes:
      - ./data/nginx:/etc/nginx/conf.d
      - ./data/certbot/conf:/etc/letsencrypt
      - ./data/certbot/www:/var/www/certbot
    ports:
      - "80:80"
      - "443:443"
    command: "/bin/sh -c 'while :; do sleep 6h & wait $${!}; nginx -s reload; done & nginx -g \"daemon off;\"'"

  certbot:
    image: tobi312/rpi-certbot
    restart: unless-stopped
    volumes:
      - ./data/certbot/conf:/etc/letsencrypt
      - ./data/certbot/www:/var/www/certbot
    entrypoint: "/bin/sh -c 'trap exit TERM; while :; do certbot renew; sleep 12h & wait $${!}; done;'"

  watchtower:
    image: v2tec/watchtower:armhf-latest
    restart: always
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /root/.docker/config.json:/config.json
    command: --interval 604800
{{< / highlight  >}}

The `docker-compose.yml` contains quite some images:

- The `postgres` one is simple: we need a database with a user;
- The `miniflux` one is configured as per their [docs];
- `ngix` is also pretty simple: we add a couple of touches: we mount the configuration folder, we
  mount the letsencrypt certificates, and the `www` folder to put the verification for letsencrypt;
- `rpi-certbot` is an ARM-ready image with [certbot]
- `watchtower` is a docker image that looks (in my case every 604800 seconds, every week) that
  all the images I'm using are up to date. If not, the image will be updated. This is especially
  relevant for `nginx` and `miniflux` which are the containers facing the users.

For `nginx` the `app.conf` file is needed. Its content is

{{< highlight nginx >}}
server {
    listen 80;
    server_name <my_domain>;
    server_tokens off;

    location /.well-known/acme-challenge/ {
        root /var/www/certbot;
    }

    location / {
        return 301 https://$host$request_uri;
    }
}

server {
    listen 443 ssl;
    server_name <my_domain>;
    server_tokens off;
    resolver 127.0.0.11;
    set $upstream service.rss:8080;

    ssl_certificate /etc/letsencrypt/live/<my_domain>/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/<my_domain>/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

    location / {
        proxy_pass  http://$upstream;
        proxy_set_header    Host                $http_host;
        proxy_set_header    X-Real-IP           $remote_addr;
        proxy_set_header    X-Forwarded-For     $proxy_add_x_forwarded_for;
        proxy_set_header  X-Forwarded-Ssl     on;
        proxy_set_header  X-Forwarded-Proto   $scheme;
        proxy_set_header  X-Frame-Options     SAMEORIGIN;

        client_max_body_size        100m;
        client_body_buffer_size     128k;

        proxy_buffer_size           4k;
        proxy_buffers               4 32k;
        proxy_busy_buffers_size     64k;
        proxy_temp_file_write_size  64k;
    }
}
{{< / highlight >}}

There's not much to explain here. The last snippet is the `init-letsencrypt.sh`. The script
"bootstraps" `nginx` for the first time: since we want `https`, but we cannot have it without
certificates, but we cannot ask certificates without a running `nginx`, this script creates fake
certificates, start `nginx`, removes the certificates, and then request real ones through
letsencrypt. The content is quite long, but here you go:

{{< highlight bash >}}
#!/bin/bash

if ! [ -x "$(command -v docker-compose)" ]; then
  echo 'Error: docker-compose is not installed.' >&2
  exit 1
fi

domains=(<my_domain>)
rsa_key_size=4096
data_path="./data/certbot"
email="" # Adding a valid address is strongly recommended
staging=0 # Set to 1 if you're testing your setup to avoid hitting request limits

if [ -d "$data_path" ]; then
  read -p "Existing data found for $domains. Continue and replace existing certificate? (y/N) " decision
  if [ "$decision" != "Y" ] && [ "$decision" != "y" ]; then
    exit
  fi
fi


if [ ! -e "$data_path/conf/options-ssl-nginx.conf" ] || [ ! -e "$data_path/conf/ssl-dhparams.pem" ]; then
  echo "### Downloading recommended TLS parameters ..."
  mkdir -p "$data_path/conf"
  curl -s https://raw.githubusercontent.com/certbot/certbot/master/certbot-nginx/certbot_nginx/options-ssl-nginx.conf > "$data_path/conf/options-ssl-nginx.conf"
  curl -s https://raw.githubusercontent.com/certbot/certbot/master/certbot/ssl-dhparams.pem > "$data_path/conf/ssl-dhparams.pem"
  echo
fi

echo "### Creating dummy certificate for $domains ..."
path="$data_path/conf/live/$domains"
mkdir -p "$data_path/conf/live/$domains"
openssl req -x509 -nodes -newkey rsa:1024 -days 1 \
  -keyout $path/privkey.pem \
  -out $path/fullchain.pem \
  -subj '/CN=localhost'
echo


echo "### Starting nginx ..."
docker-compose up --force-recreate -d nginx
echo

echo "### Deleting dummy certificate for $domains ..."
docker-compose run --rm --entrypoint "\
  rm -Rf /etc/letsencrypt/live/$domains && \
  rm -Rf /etc/letsencrypt/archive/$domains && \
  rm -Rf /etc/letsencrypt/renewal/$domains.conf" certbot
echo


echo "### Requesting Let's Encrypt certificate for $domains ..."
#Join $domains to -d args
domain_args=""
for domain in "${domains[@]}"; do
  domain_args="$domain_args -d $domain"
done

# Select appropriate email arg
case "$email" in
  "") email_arg="--register-unsafely-without-email" ;;
  *) email_arg="--email $email" ;;
esac

# Enable staging mode if needed
if [ $staging != "0" ]; then staging_arg="--staging"; fi

docker-compose run --rm --entrypoint "\
  certbot certonly --webroot -w /var/www/certbot \
    $staging_arg \
    $email_arg \
    $domain_args \
    --rsa-key-size $rsa_key_size \
    --agree-tos \
    --force-renewal" certbot
echo

echo "### Reloading nginx ..."
docker-compose exec nginx nginx -s reload
{{< / highlight >}}

For this and for `app.conf` file I took inspiration from the [nginx-certbot] repository with some
modification: I'm using `rpi-certbot` instead of `certbot` and the `openssl` utility that comes
with the Raspberry (if it doesn't, use `sudo apt-get install openssl` to get it).

## Outside the Raspberry

The outside world need to know where to find your Raspberry and should be able to get there. Doing
so is outside the scope of this post, but in general

- Find your external IP address (and hope it's static or use a service such as [Dyn])
- Update the DNS of your (sub)domain (For example `reader.lanzani.nl`)
- Assign a static DNS to your Raspberry from your router
- Route port `80` and `443` in your router so that the traffic is handled by the Raspberry (port
  `80` is necessary for the letsencrypt verification).

## Start everything

Once all these files are in place, you are in the right folder, and you have updated the various
variables marked with `<>` (passwords and domain name) in the files above you can get rolling with

```sh
curl -sSL https://get.docker.com | sh
pip install --user docker-compose
bash init-letsencrypt.sh
docker-compose up
```

Now visit your (sub)domain, use `admin` as the user and the password you have chosen to log in.
Enjoy the rest!


[RSS]: https://en.wikipedia.org/wiki/RSS
[shut down]: https://en.wikipedia.org/wiki/Google_Reader#Discontinuation
[Stringer]: https://github.com/swanson/stringer
[Unread]: https://www.goldenhillsoftware.com/unread/
[south]: https://www.google.com/search?hl=en&q=corrupted%20sd%20card%20raspberry
[scenarios]: https://nickjanetakis.com/blog/docker-tip-34-should-you-use-docker-compose-in-production
[Docker]: https://www.docker.com
[docker-compose]: https://docs.docker.com/compose/
[Pipelines]: https://azure.microsoft.com/en-us/services/devops/pipelines/
[buildx]: https://github.com/docker/buildx
[miniflux]: https://miniflux.app
[certbot]: https://certbot.eff.org
[Dyn]: https://dyn.com
[docs]: https://miniflux.app/docs/installation.html#docker
[Fever]: https://feedafever.com/api
[nginx-certbot]: https://github.com/wmnnd/nginx-certbot
