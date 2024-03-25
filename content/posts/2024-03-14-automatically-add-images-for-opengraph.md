---
date: 2024-03-14
title: Automatically add images for Open Graph in Hugo
snippet: |
   While using a static site generator is a low-maintenance endeavor, it also means that complex requirements need to be coded if nobody has done it before.

   Today, I automated image previews that can be used to display a nice preview through for Open Graph (and Twitter cards!).
url: /2024/opengraph-hugo
---
While using a static site generator is a low-maintenance endeavor, it also means that complex requirements need to be coded if nobody has done it before.
Today, I automated image previews that can be used to display a nice preview through for Open Graph (and Twitter cards!).

The [Open Graph] protocol enables any web page to become a rich object in a social graph. My blog, however, is text-heavy and often misses the images, so I had to come up with something else.

In the end, I settled with a solution (code below) that displays a static image with some text on top, including the title, a snippet of the blog, and the URL.

The snippet can be explicitly set in the front matter with the `snippet` key, otherwise it will take the first characters of the post itself.

To make it all work, you need a couple of things in your Hugo theme

{{< highlight sh >}}
├── assets
│  ├── Inter-Medium.ttf
│  ├── Inter-SemiBold.ttf
│  └── og_base.png
├── layouts
│  ├── partials
│  │  ├── opengraph.html
   
{{< / highlight >}}

Here, `og_base.png` is just the empty image used to write text on, like:

{{< figure src="/images/og_base.png" width="90%" >}}

The fonts are just the fonds, and `opengraph.html` contains:

{{< gist gglanzani d728122fc9925db9fd1eaec7c99dc484 >}}


In the, then, every blog post will be accompanied by an image as

{{< figure src="/2024/open-graph-hugo/og.png" width="90%" >}}

[Open Graph]: https://ogp.me/
