---
date: 2024-01-09T00:00:00Z
title: Mark Apple Music song as favorite in the background
tags: ["apple music", "applescript", "keyboard maestro", "launchbar", "productivity", "technology"]
url: /2024/favorite-apple-music
---

I often listen to Apple Music in the background and sometimes I casually listen to a song I want to add to my favorites.

Instead of jumping around, I created a small AppleScript script (that can be executed through [Launchbar] or [Keyboard Maestro]).

I report it here for posterity!

{{< highlight applescript >}}
tell application "Music"
	set favorited of current track to true
end tell
{{< / highlight >}}


[Launchbar]: https://www.obdev.at/products/launchbar/index.html
[Keyboard Maestro]: https://www.keyboardmaestro.com/main/
