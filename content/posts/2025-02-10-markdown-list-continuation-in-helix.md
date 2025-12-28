---
title: Markdown List Continuation in Helix
tags: ["helix", "markdown", "productivity", "programming", "technology review", "toml", "tutorial"]
author: Giovanni Lanzani
date: 2025-02-10T00:00:00Z
snippet: |
  One of the gripes I have with Helix is not having automatic list continuation when writing Markdown.

  Until last week, when I found out that there's a clever trick to achieve it
url: /2025/markdown-list-continuation-in-helix
---

One of the gripes I have with [Helix] is not having automatic list continuation when writing Markdown.

Until last week, when I found out that there's a clever [trick] to achieve it, namely adding this to `~/.config/helix/languages.toml`

{{< highlight toml >}}
[[language]]
name = "markdown"
comment-tokens = ["-", "+", "*", "1.", ">", "- [ ]"]
{{< /highlight >}}

Note that when you have an ordered list, the trick will put the same number (e.g., **1.**) on each line. That's not a problem though because they will still be rendered properly with ascending numbers.

[Helix]: https://helix-editor.com/
[trick]: https://github.com/helix-editor/helix/discussions/12812
