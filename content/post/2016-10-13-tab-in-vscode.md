---
date: 2016-10-13T00:00:00Z
title: Use tab to cycle through Visual Studio Code completion
url: /2016/tab-in-vscode/
---

Sometimes, instead of using [NeoVim], I do like to use 
[Visual Studio Code](https://code.visualstudio.com) (with Vim keybindings).

Visual Studio Code is a great editor with amazing Intellisense and debugging capabilities (for 
Python as well). There is however one thing that I could not swallow: the tab behavior when a 
completion was suggested.

With (Neo)Vim `shift+tab` and `tab` respectively cycle up and down the completion list.

I wanted to have the same experience in Visual Studio Code. After some Googling a lot of trial and
errors, this is what I came up with (it works pretty nicely! You can paste the text below in the
file that is opened after you click on `File -> Preferences -> Keyboard Shortcuts`)

{{< highlight json >}}
[
        {
        "key": "tab",
        "command": "selectNextQuickFix",
        "when": "editorFocus && quickFixWidgetVisible"
    },
        {
        "key": "shift+tab",
        "command": "selectPrevQuickFix",
        "when": "editorFocus && quickFixWidgetVisible"
    },
        {
        "key": "tab",
        "command": "selectNextSuggestion",
        "when": "editorTextFocus && suggestWidgetMultipleSuggestions && suggestWidgetVisible"
    },
        {
        "key": "shift+tab",
        "command": "selectPrevSuggestion",
        "when": "editorTextFocus && suggestWidgetMultipleSuggestions && suggestWidgetVisible"
    }
]
{{< / highlight >}}

The commands should be pretty self-explanatory! To accept the suggestion, you can use `enter` 
(that's the default together with `tab`). 


[NeoVim]: https://github.com/neovim/neovim/