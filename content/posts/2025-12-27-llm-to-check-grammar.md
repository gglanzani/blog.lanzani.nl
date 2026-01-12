---
title: LLMs for grammar
author: Giovanni Lanzani
date: 2025-12-27T21:00:00Z
tags: ["Keyboard Maestro", "LLM", "automation", "writing"]
snippet: |
  Today, Dr. Drang asked about my setup to leverage LLMs to check my grammar. So, I had to write it up!
url: /2025/llms-for-grammar
---

UPDATE: There's a more refined version of this in a new [post](/2026/a-diff-based-llm-powered-grammar-check).

Today, [Dr. Drang](https://fosstodon.org/@drdrang) asked on [Mastodon](https://fosstodon.org/@drdrang/115793520510254515) about my setup to leverage LLMs to check my grammar.

My setup has a couple of assumptions, just like his:

- I have a native macOS app holding text[^1]
- I write in markdown (although this one can be relaxed)

### Tooling

- I use [Keyboard Maestro](https://www.keyboardmaestro.com/main/)
- I use the excellent [llm](https://datasette.io/tools/llm) command-line utility

For this post, I'm assuming you already set up `llm`, but if not and you have [uv](https://docs.astral.sh/uv/) handy, it's this simple:

```
uv tool install llm
llm keys set openai # if using OpenAI
# if using gemini
llm install llm-gemini 
llm keys set gemini
# if using Claude
llm install llm-anthropic
llm keys set anthropic
```

### The macro

Once this is in place, the macro is pretty straightforward, and I invoke it with the text I want to check **selected**. I then have to wait a bit, and the script will replace the text with a corrected version, each corrected item marked in bold (I don't use bold much in my text, so that is OK).

{{< image
  src="images/check-grammar.jpg"
  alt="A screenshot of the Keyboard Maestro macro to check for grammatical mistakes"
  title="A screenshot of the Keyboard Maestro macro to check for grammatical mistakes"
>}}

The core of the macro is the (fish, but it will work on other shell unmodified, probably) script (pasted here to make it easier to copy and paste):

```fish
#!/opt/homebrew/bin/fish

pbpaste | llm -m gemini-3-flash-preview -s "
Given the following text, improve grammar and punctuations. 

Once you're done, highlight the changes by using a double asterisk (bold, in Markdown). Only output the text, don't add commentary or summaries anywhere in the text" 
```

Since the macro highlights clearly what has been changed, I can learn a bit as well! And if the window loses focus, everything will be in the clipboard.

[^1]: Not all text might need to be checked, though. For example, in my mail client, most of the text I'm replying to doesn't need a grammar check.
