---
title: pip introduced dependency cooldowns
author: Giovanni Lanzani
date: 2026-04-28T00:00:00Z
url: /2026/pip-introduced-dependency-cooldowns
---

In a previous [post], I described how `uv` can now protect you from supply-chain attacks with the `exclude-newer` feature.

This week, [pip] introduced a similar feature, `--uploaded-prior-to`. The simple syntax is:

```sh
python -m pip install --uploaded-prior-to=P<n_days>D <my_package>
```

In practice, to install a package ([litellm] in this example) excluding releases newer than a week:

```sh
python -m pip install --uploaded-prior-to=P7D litellm
```

A great step to increase security!

[post]: /2026/protect-against-supply-chain-exploits-in-uv
[pip]: https://pip.pypa.io/en/stable/
[litellm]: https://pypi.org/project/litellm/
