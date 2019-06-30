---
date: 2019-06-30T00:00:00Z
title: Netlified
url: /2019/netlified
---

A couple of days ago I moved the blog and the website over to [Netlify].

The reasons are simple:

- The site was previously hosted on S3 + Cloudfront, but I didn't have `https` enabled;
- I didn't know how to enable `https` although I must not be too hard;
- The application I was using to deploy to S3 — called [Stout] — was unmaintained and growing old.

Every time I've read comments on Netlify the message was the same: it's easy to set up, and once
you've set it up, you can forget about it.

I gave myself 5' to try: if I could do it, good, otherwise I would stay on the current setup.

Well, not only I could do it, but the whole project was undistinguishable from magic. They took
care of everything for `blog.lanzani.nl` and `lanzani.nl`, including serving the naked domain
(previously it would be forwarded to `www.lanzani.nl`, something that always bothered me).

As they integrate with [GitHub] and [hugo], I don't even need to build the website anymore, they do
it for me every time I push the repo!

So the end result is that you can read this blog without fearing that someone has tampered with the
content!


[Netlify]: https://www.netlify.com
[stout]: https://github.com/cloudflare/Stout
[hugo]: https://gohugo.io
[GitHub]: https://github.com/gglanzani/blog.lanzani.nl
