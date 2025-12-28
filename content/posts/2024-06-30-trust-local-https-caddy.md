---
date: 2024-06-30T00:00:00Z
title: Trust local caddy certificates on macOS
tags: ["caddy", "certificates", "docker", "macos", "programming", "technology", "tutorial"]
url: /2024/trust-local-caddy-certificates-on-macos
---

Up to today, I've been bothered by having local https websites served by [Caddy], whose certificates were not trusted by macOS. Today, I rectified it.

For macOS (and Safari) to trust what Caddy deploys locally, I had to:

- Find the root certificate (if you're using the Docker image, they're in `/data/caddy/pki/authorities/local/root.crt`).
- Copy its content into, say, `caddy.pem` (it's just a name that the Keychain Access understands and that won't clutter it with a non-informative name as `root.crt`)
- Double-click `caddy.pem` to open it in the Keychain Access.app
- Double-click the certificate name, open up the *Trust* "tab", and click on "Always Trust".

{{< figure src="/images/keychain_trust.png" width="90%" alt="A screenshot of a certificate open with Keychain Access" >}}

Then, all the local websites Caddy is serving will be trusted automatically.

[Caddy]: https://caddyserver.com/
