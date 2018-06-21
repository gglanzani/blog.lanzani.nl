---
date: 2010-02-03T00:00:00Z
title: Background color with Skim
url: /2010/Background-color-with-Skim/
---

Today I stumbled upon a Lifehacker article which explained how to change the background color in Adobe Reader, when displaying PDF (which could alleviate the eyes’ stress if reading a lot). Well, it turn out that the same can be accomplished with my favorite OS X pdf reader, Skim (from version 1.3.4 on). Just open Skim, then fire up Apple Script editor, and paste the following code

{{< highlight applescript >}}
tell application "Skim"
set theColor to choose color
set page background color to theColor
end tell
{{< / highlight >}}

It is not a great hassle to do it once, but if you plan to do it more frequently, you can save the script as an application, and then drag it to the Finder toolbar, so that you will always have it available

