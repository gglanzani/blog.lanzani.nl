---
title: Hey Claude, delete my home folder
tags: ["ai agents", "anthropic claude cli", "gemini cli", "openai codex", "programming", "technology review", "user experience"]
author: Giovanni Lanzani
date: 2025-12-09T00:00:00Z
url: /2025/hey-claude-delete-my-home-folder
link: https://www.reddit.com/r/ClaudeAI/comments/1pgxckk/claude_cli_deleted_my_entire_home_directory_wiped/ 
link_title: Claude CLI deleted my entire home directory! Wiped my whole mac.
snippet: |
  Use agents, they said. You will save time, they said.
---

Use agents, they said. You will save time, they said.

Tools like Anthropic Claude CLI, OpenAI Codex, Gemini CLI, etc. can be super useful but, by default, they don't execute commands before asking for permission each time.

As a result, multi-steps tasks can become tedious to validate each time. You need to keep constant attention to it, track what it does, approving all changes and actions.

However, there's a way around it, the `--dangerously-skip-permissions` flag. Using that flag, the agent starts doing things in the background and you can go on with your life and check back once it's done.
But agents hallucinate, and, sometimes, they do so big time. Like what happened with [LovesWorkin](https://www.reddit.com/user/LovesWorkin/) (username checks out) on Reddit: Claude run a `rm -rf tests/ patches/ plan/ ~/` command, wiping their whole home folder 😱

The comments, as always, are a joy to read.


{{< figure src="https://preview.redd.it/claude-cli-deleted-my-entire-home-directory-wiped-my-whole-v0-egjqmw80bv5g1.png?width=464&format=png&auto=webp&s=9b147aefd85dcb3390b8260cfcd4c639c5e272ee" alt="A screenshot of the terminal showing the tasks Claude was executing" >}}


