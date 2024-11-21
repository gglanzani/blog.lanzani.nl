---
date: 2024-11-15T00:00:00Z
title: Linking Directly to Web Page Content with Typinator
snippet: |
  Recently, browsers added the ability to go directly go to a webpage highlighting a particular piece of text.

  So, I set out to automate the process of creating these links using Typinator!
url: /2024/getting-links-with-highlights-automatically
---

Recently, the major browsers [introduced] the capability to go to a webpage highlighting and scrolling to a particular piece of text. Once you see it in action, it really is neat.

They syntax is `<original_url>#:~:text=` followed by the URI encoded text you want to highlight, like [https://blog.lanzani.nl/2024/who-will-speak-up-for-free-speech/#:~:text=Economist](https://blog.lanzani.nl/2024/who-will-speak-up-for-free-speech/#:~:text=Economist) (you can click on it to see for yourself).

Typing the whole thing is, however, cumbersome, so I resorted to [Typinator] to automate some of it, creating a snippet (bound to `;frag`) like this:

{{< highlight javascript "linenos=inline" >}}
{/JavaScript
let text = encodeURIComponent("{clip}")
let safari = Application("Safari");
safari.documents[0].url() + "#:~:text=" + text
}
{{< / highlight >}}

The way I engage with it is simple: on a page, I copy some text, and then, in another application (it doesn't work in Safari—although with some tweaks it might) I type `;frag` and the URL including the `#:~text=something` appears instead.

The script works as follows:

1. Line 2 expands the clipboard and URI encodes it (for example, replacing spaces with `%20`).
2. Line 3 grabs an object that lets you manipulate the Safari application. This Javascript system built into the system is one of those big details in macOS that sets it apart from the competition, in my opinion.
3. Line 4 gets the URL of the active tab, and appends first `#:~:text=` and then the encoded text.

That's it. Simple and effective.

[Typinator]: https://www.ergonis.com/products/typinator/index.html
[introduced]: https://alfy.blog/2024/10/19/linking-directly-to-web-page-content.html
