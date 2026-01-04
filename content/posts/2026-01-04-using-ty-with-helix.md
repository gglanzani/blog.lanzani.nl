---
title: Using ty with the helix editor
author: Giovanni Lanzani
date: 2026-01-04T01:00:00Z
snippet: |
   How to set up ty, Astral's type checker and language server, with Helix.
tags: ["helix", "python", "ty", "tutorial"]
url: /2026/using-ty-with-the-helix-editor
---

The folks at [Astral] have released [ty], which they define as:

> An extremely fast Python type checker and language server, written in Rust.

You can read more in the blog [post] announcing it.

Today, I set it up with [helix]. To make it work, we first have to install it. Using `uv`, it's as easy as typing:

```sh
uv tool install ty@latest
```

Then, edit your `~/.config/helix/languages.toml` and add the following:

```toml
[language-server.ty]
command = "ty"
args = ["server"]

[[language]]
name = "python"
scope = "source.python"
injection-regex = "python"
file-types = ["py","pyi","py3","pyw",".pythonstartup",".pythonrc"]
shebangs = ["python"]
roots = [".", "pyproject.toml"]
comment-token = "#"
language-servers = ["ty"]
indent = { tab-width = 4, unit = "    " }
auto-format = true
```

At this point, you can start using it, but there are a few handy things to know:

- By default, `ty` will search for first-party modules in the project's root directory or the `src` directory, if present.
- If they are located elsewhere, you need to update your `pyproject.toml` like so:
```toml
[tool.ty.environment]
root = ["./app"]
```
- Third-party modules are searched for within a virtual environment, determined by:
  1. An environment variable called `VIRTUAL_ENV`.
  2. A `.venv` directory in the project root or working directory.
  3. The `--python` or `--venv` flag when invoking it.
  4. A configuration in `pyproject.toml`:
```toml
[tool.ty.environment]
python = "./custom-venv-location/.venv"
```

That's it. Now, when editing files with `hx`, you should get all of `ty`'s goodness!

[Astral]: https://astral.sh/
[ty]: https://docs.astral.sh/ty/
[post]: https://astral.sh/blog/ty
[helix]: https://helix-editor.com/
