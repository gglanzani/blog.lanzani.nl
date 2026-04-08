---
title: Get Rid of Bartender and Other Menu Bar Apps
author: Giovanni Lanzani
date: 2026-04-08T00:00:00Z
url: /2026/get-rid-of-bartender-and-other-menu-bar-apps
tags:
  - mac
  - technology
  - tutorial
---

Since having a notch on my MacBook, I cannot fit all the icons in my menu bar.

I've tried heaps of apps to make it work: [Bartender], [Ice]/[Thaw], and [Hidden], but they all had subtle bugs.

Turns out, there's an easy way to avoid using these apps by shrinking the spacing and padding of the menu bar items, and now I don't need any of these apps!

From the terminal, type 


```sh
defaults -currentHost write -globalDomain NSStatusItemSpacing -int 2
defaults -currentHost write -globalDomain NSStatusItemSelectionPadding -int 2
killall ControlCenter
```

(You can play around with `-int 2` and see what works best for you!).

To undo and go back to normal, type

```sh
defaults -currentHost delete -globalDomain NSStatusItemSpacing
defaults -currentHost delete -globalDomain NSStatusItemSelectionPadding
killall ControlCenter
```

It's a bit crammed, but it's bug-free.

[Bartender]: https://www.macbartender.com/
[Ice]: https://github.com/jordanbaird/Ice
[Thaw]: https://github.com/stonerl/Thaw
[Hidden]: https://github.com/dwarvesf/hidden
