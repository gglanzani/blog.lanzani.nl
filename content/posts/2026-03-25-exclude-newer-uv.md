---
title: Protect against supply-chain exploits using uv
author: Giovanni Lanzani
date: 2026-03-25T00:00:00Z
url: /2026/protect-against-supply-chain-exploits-in-uv
tags:
  - python
  - security
  - uv
---

[LiteLLM] was recently [victim] of a supply-chain [exploit], where an attacker was able to run arbitrary code on infected machines.

In the aftermath, I saw how [uv] provides a safety setting for this, and it would be good practice to add this to your `pyproject.toml` 

```toml
[tool.uv]
exclude-newer = "1 week"
```

or `uv.toml`:

```toml
exclude-newer = "1 week"
```

The [docs] provide multiple options to protect yourself.

[docs]: https://docs.astral.sh/uv/reference/settings/#exclude-newer
[victim]: https://news.ycombinator.com/item?id=47501426
[exploit]: https://en.wikipedia.org/wiki/Supply_chain_attack
[LiteLLM]: https://github.com/BerriAI/litellm/
[uv]: https://github.com/astral-sh/uv
