---
date: 2023-11-23T00:00:00Z
title: Use the Grammarly language server with helix
tags: ["configuration", "grammarly", "helix", "language server", "markdown", "programming", "tutorial"]
url: /2023/grammarly-helix-lsp
---

Lately, I've been playing around with [helix], a relatively new modal editor.

One of the things I was missing was an [LSP] for [Grammarly] (the only reason I can put the commas where they belong).

After struggling a bit with the configuration, I've finally found what works 

```toml
[[language]]
name = "markdown"
auto-format = false
file-types = [ "markdown", "md", "mdown", "txt" ]
language-servers = [ "grammarly" ]

[language-server.grammarly]
command = "grammarly-languageserver"
args = ["--stdio"]
config = { clientId = "client_BaDkMgx4X19X9UxxYRCXZo"}
```

Note that:

- You need to install the Grammarly language [server] before.
- `clientId` is not a secret and is the same for all installations.

[helix]: https://helix-editor.com/
[LSP]: https://langserver.org/
[Grammarly]: https://www.grammarly.com/
[server]: https://github.com/znck/grammarly
