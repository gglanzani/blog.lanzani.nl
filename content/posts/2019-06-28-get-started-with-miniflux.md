---
date: 2019-06-28T00:00:00Z
title: Get started with miniflux
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

{{< gist gglanzani 40ff6d2cbc22c6761ed29630eee6ecd0 "docker-compose.yml" >}}

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

{{< gist gglanzani 40ff6d2cbc22c6761ed29630eee6ecd0 "app.conf" >}}

There's not much to explain here. The last snippet is the `init-letsencrypt.sh`. The script
"bootstraps" `nginx` for the first time: since we want `https`, but we cannot have it without
certificates, but we cannot ask certificates without a running `nginx`, this script creates fake
certificates, start `nginx`, removes the certificates, and then request real ones through
letsencrypt. The content is quite long, but here you go:

{{< gist gglanzani 40ff6d2cbc22c6761ed29630eee6ecd0 "init-letsencrypt.sh" >}}

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
