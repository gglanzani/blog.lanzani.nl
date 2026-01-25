---
title: Slack's Feed Command is Secretly Broken
author: Giovanni Lanzani
date: 2026-01-24T00:00:00Z
url: /2026/slack-feed-command-is-secretly-broken
tags: ["slack", "RSS", "XML"]
snippet: |
  The ability to subscribe to feeds directly from Slack is great, until it isn't.
---

Slack's `/feed` [command] is really useful; it allows you to subscribe to an XML feed and receive updates in any channel.

However, it doesn't always work as expected, and when it fails, there's no way to find out why.

Recently, I added an [Atom] feed to an [announcement] channel, but the posts weren't coming through. I initially thought the `/feed` functionality might be incompatible with announcement channels, but I was wrong.

After contacting support, they told me:

<article>
<p>
We only recognise the following tags for date values: <code>&lt;pubDate&gt;</code>, <code>&lt;issued&gt;</code>, <code>&lt;modified&gt;</code>, <code>&lt;updated&gt;</code>, <code>&lt;dc:date&gt;</code>, or <code>&lt;published&gt;</code> (this last date type applies to Atom feeds) and the date tag fields must be within an <code>&lt;item&gt;</code> tag to be read
</p>
</article>

Great, I thought. Since I controlled the Atom feed, I figured I could just wrap every entry in an `<item>` tag. However, the Atom standard expects items to be wrapped in `<entry>` instead. The spec says feeds should be like:

```xml
<feed>
  <entry>
    <title>Post Title</title>
    <published>2025-01-24T10:00:00Z</published>
  </entry>
</feed>
```

Essentially, Slack's requirements and the Atom spec are incompatible. 

In the end, since I was able to rewrite the feed, I simply switched it to RSS to get everything working.

[atom]: https://en.wikipedia.org/wiki/Atom_(web_standard)
[announcement]: https://slack.com/blog/transformation/managing-slack-at-scale-announcement-channels-and-new-admin-apis
[command]: https://slack.com/help/articles/218688467-Add-RSS-feeds-to-Slack
[RSS]: https://en.wikipedia.org/wiki/RSS
