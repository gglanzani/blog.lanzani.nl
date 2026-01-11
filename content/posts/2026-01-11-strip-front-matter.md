---
title: Strip Front Matter from YAML/TOML files
author: Giovanni Lanzani
date: 2026-01-11T00:00:00Z
url: /2026/strip-front-matter-from-yaml/toml-files
snippet: |
  Creating a mini command-line utility to strip front matter from markdown files
tags: ["markdown", "writing", "LLM", "automation"]
---

I recently started using [Marked 2] again with some of my markdown files.

The app works great and it can strip YAML from the [front matter] of the files.

However, I recently started another blog with TOML front matter, and Marked didn't handle those.

Luckily, it has a preference where you can set a custom preprocessor for your files:

![Marked 2 Advanced Preferences Pane](images/2026/Marked-2-Advanced-Preferences.webp "Marked 2 Advanced Preferences Pane")

I fired up Claude and whipped up the tiniest [utility] to do just that.

If you want to use it, it's pretty easy to compile following the `README.md` or by heading to the [releases] page. 

[Marked 2]: https://marked2app.com/
[front matter]: https://docs.github.com/en/contributing/writing-for-github-docs/using-yaml-frontmatter
[utility]: https://github.com/gglanzani/strip-front-matter
[releases]: https://github.com/gglanzani/strip-front-matter/releases/tag/v2026.01
