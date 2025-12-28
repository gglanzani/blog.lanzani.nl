---
date: 2022-02-11T07:00:00Z
title: Disable Bluetooth on Mac before Sleep
tags: ["bluetooth", "homebrew", "launchbar", "macos", "monterey", "tech tips", "tutorial"]
url: /2022/disable-bluetooth
---

The recent Monterey update (12.2), introduced a [bug] that drains the battery of my laptop while
sleeping.

A fix is to disable bluetooth before putting it to sleep, but who remembers that?

Luckily, I use [Launchbar] to put the Mac to sleep: it has a very convenient *Sleep* action.

I then copied and updated the action to have it turn off bluetooth before sleeping.

How can you do the same?

First, install [Homebrew].

Then, activate Launchbar (⌘ Space on my Mac), and then launch its index (⌥ ⌘ I).

In the general section on the left, click on Action. From there, use the search bar to find the
*Sleep* action, disable its checkbox, right-click, and select *Show in Action Editor*.

<img class="pure-img" src="/images/launchbar-action.png">

Then right-click the *Sleep* action and duplicate it.

Once you have duplicated, rename it to *Sleep BT* (or whatever), click on *Scripts*, and click on
*Edit*.

<img class="pure-img" src="/images/launchbar-sleep.png">

Replace the content of the script with

{{< highlight applescript >}}
-- Sleep
-- LaunchBar Action
-- default.scpt
-- Version 3
--
-- Copyright (c) 2007-2016 Objective Development
-- https://obdev.at/

tell application "LaunchBar" to hide
delay 0.5
do shell script "/usr/local/bin/ blueutil -p 0"
tell application "System Events" to sleep
{{< / highlight >}}

The only new line is the one but last, `do shell script "/usr/local/bin/ blueutil -p 0"`.

Save it, and you're done!

Remember though: every time you wake the Mac up from sleep, you need to reactivate bluetooth!

[bug]: https://www.macworld.com/article/610175/macos-monterey-beta-battery-drain-bluetooth.html
[Launchbar]: https://www.obdev.at/products/launchbar/index.html
[Homebrew]: https://brew.sh
