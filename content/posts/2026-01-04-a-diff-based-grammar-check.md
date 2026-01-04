---
title: A diff-based LLM-powered grammar check
author: Giovanni Lanzani
date: 2026-01-04T00:00:00Z
snippet: |
  
url: /2026/a-diff-based-llm-powered-grammar-check
---

Since [posting] about how I use `llm` to check my grammar, Dr. Drang took inspiration from my setup and [wrote] about how he implemented a system that shows differences side-by-side, instead of highlighting them as I do. On top of that, his system includes a short rationale for the changes.

I have always liked the idea of a diff-based check, but because I write in [Mailmate], [helix], and the browser, I couldn't come up with a system that worked everywhere.

That was until I remembered about [Meld], a diffing app.

Before starting, it's important to have Meld installed and available at the command line (to do so, install it via `brew install meld` and follow these [instructions]).

Once that is done (and you have `llm` installed—see my previous [post]), I use this macro that I invoke after **copying** the text:

![The short Keyboard Maestro macro to use an LLM to check for my grammar](images/keyboard-maestro-meld.webp)

The script does all the heavy lifting and is self-explanatory:

```fish
#!/opt/homebrew/bin/fish

pbpaste > /tmp/original.md
pbpaste | llm -m gemini-3-flash-preview -s "
Given the following text, improve grammar and punctuations. Note that there might be markdown syntax that needs to be preserved

Output the text and add commentary only at the end, after a clear break line (use dashes)" > /tmp/corrected.md
cat /tmp/corrected.md | pbcopy
meld /tmp/original.md /tmp/corrected.md
```

Once invoked, Meld opens, allowing me to review the changes and copy the text back:

![The Meld main window showing this post and the uncorrected version of it](images/meld-main.webp "The interface to see the corrections")

If you write long lines of text without hard wraps, don't forget to check the *Enable text wrapping* box in the preferences pane!

![Meld Preferences pane](images/meld-preferences.webp)

[posting]: /2026/a-diff-based-llm-powered-grammar-check/
[post]: /2026/a-diff-based-llm-powered-grammar-check/
[wrote]: https://leancrew.com/all-this/2026/01/proofreading-with-the-claude-api/
[Mailmate]: https://freron.com/
[helix]: https://helix-editor.com/
[Meld]: https://meldmerge.org/
[instructions]: https://gitlab.com/dehesselle/meld_macos#in-the-terminal
