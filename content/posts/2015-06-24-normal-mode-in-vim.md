---
date: 2015-06-24T00:00:00Z
title: Normal mode in Vim
tags: ["productivity", "programming", "sql", "technology-review", "text-editing", "vim"]
url: /2015/normal-mode-in-vim/
---

I consider myself an advanced Vim user. But I always found annoying the multiple cursors support
that Vim has (through [vim-multiple-cursors]) and below the [Atom] and [Sublime] standards.

For example, say I have this piece of SQL and I want to convert all the field names to lowercase:

{{< highlight sql "linenos=inline" >}}

CREATE TABLE test (
MYFIELD1 STRING,
MYFIELD2 STRING,
...
MYFIELDN STRING
)

{{< / highlight >}}

Doing it with macros it tedious, and with multiple cursor, at least in Vim, it can be annoying,
especially if you have lots of tables create like that, spanning multiple lines.

## Enter normal mode
Normal mode comes to the rescue here: you just need to know which commands you would execute on one
line to get the desired result (in this case `_g~w`) and the range of lines where the commands
should be executed (in this case `2,5`). Then it's just a matter of typing `:2,5norm _g~w` and,
almost magically, the code will be transformed in what we wanted!
{{< highlight sql "linenos=inline" >}}
CREATE TABLE test (
myfield1 STRING,
myfield2 STRING,
...
myfieldn STRING
)
{{< / highlight >}}
This is one of the features that keeps me in Vim and not in one of the emulated Vim mode out there
(like Atom Vim mode)

[vim-multiple-cursors]: https://github.com/terryma/vim-multiple-cursors
[Atom]: https://atom.io
[Sublime]: https://sublimetext.com

