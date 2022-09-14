---
date: 2022-09-14T00:00:00Z
title: Configure Caddy v2 to reverse proxy the Unifi Controller
url: /2022/caddy-v2-and-unifi
---

[Caddy] is an open source web server that can be used to, among others, proxy other sorts of server adding https with valid certificates.

At home, I have a Unifi controller that uses https but has no valid certificate, so I decided to expose it through caddy.

Since I run caddy as an unpriviliged user, it listens to port 2016 for http and 2017 for https. My router then listens to port 80 / 443 and reroutes to port 2016 / 2017 on the host running caddy.

The working configuration I came up with my Caddyfile is pretty simple

{{< highlight caddyfile >}}
{
  http_port 2016
  https_port 2017

  unifi.lanzani.nl {
    reverse_proxy 127.0.0.1:8443 {  # the unifi controller runs on the same machine as caddy
      transport http {
        tls_insecure_skip_verify  # we don't verify the controller https cert
      }
      header_up - Authorization  # sets header to be passed to the controller
    }
  }
}
{{< / highlight >}}

That's it!


[Caddy]: https://caddyserver.com
