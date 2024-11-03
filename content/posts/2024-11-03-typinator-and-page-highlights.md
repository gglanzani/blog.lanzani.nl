---
date: 2024-11-03T00:00:00Z
title: Getting links with highlights automatically
url: /2024/getting-links-with-highlights-automatically
---

Recently, the major browsers introduced the capability to go to a webpage highlighting and scrolling to a particular piece of text. Once you see it in action, it really is neat.

They syntax is `<original_url>#:~:text=` followed by the URI encoded text you want to highlight, like [https://blog.lanzani.nl/2024/who-will-speak-up-for-free-speech/#:~:text=Economist](https://blog.lanzani.nl/2024/who-will-speak-up-for-free-speech/#:~:text=Economist) (you can click on it to see for yourself).

Typing the whole thing is, however, cumbersome, so I resorted to [Typinator] to automate some of it, creating a snippet (bound to `;frag`) like this:

{{< highlight javascript >}}
{/JavaScript
let text = encodeURIComponent("{clip}")
let safari = Application("Safari");
safari.documents[0].url() + "#:~:text=" + text
}
{{< / highlight >}}

The way I engage with it is simple: on a page, I copy some text, and then, in another application (it doesn't work in Safari—although with some tweaks it might) I type `;frag` and the URL including the `#:~text=something` appears instead.

The script works as follows:

1. The second line expands the clipboard and URI encodes it (for example, replacing spaces with `%20`).
2. The third grabs an object that lets you manipulate the Safari application. This Javascript system built into the system is one of those big details in macOS that sets it apart from the competition, in my opinion.
3. The fourth gets the URL of the active tab, and appends first `#:~:text=` and then the encoded text.

That's it. Simple and effective.


[Typinator]: https://www.ergonis.com/products/typinator/index.html
