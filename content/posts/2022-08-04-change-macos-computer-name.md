---
date: 2022-08-04T07:00:00Z
title: Change macOS computer name
url: /2022/change-computer-name-macOS
---

I recently upgraded to a new M1 Pro Macbook Pro and the computer is managed by the company.

It means that — for one reason or the other, spuriously documented on Apple discussion [forum] — I was not able to change the computer name.

<img class="img-responsive" alt="can't change computer name on macOS" src="https://discussions.apple.com/content/attachment/562718040">

A good soul documented the solution, that I am reporting here for my future self: fire up the terminal and type

{{< highlight sh >}}
sudo scutil --set ComputerName <your_name_here>
{{< / highlight >}}

Voilà!

[forum]: https://discussions.apple.com/thread/7009597
