---
date: 2011-04-08T00:00:00Z
title: Growl.log and Perl
url: /2011/growl_log-and-perl/
---

I really like [Growl](http://growl.info/). It's the kind of thing that
'just works', and they do it nicely. It tells me when someone send me
a message when I'm not present, or updates some files in my Dropbox
shared folders. Or when I receive a new Pinboard bookmark. And lot of
other things. You may not like the number of notifications, but you
can customize it.

The point is, the notifications go away after a certain amount of
time. They may have been important, but if I don't read them
immediatly, they're gone from my desktop. Luckily there's the
`Growl.log` file. It is basically a file which reports any Growl
activity. To enable it, type in the terminal (change `YOUR_USERNAME`
with your username)

{{< highlight bash >}}
touch ~/Library/Logs/Growl.log
defaults write com.Growl.GrowlHelperApp GrowlLoggingEnabled -bool YES
defaults write com.Growl.GrowlHelperApp GrowlLogType 1
defaults write com.Growl.GrowlHelperApp "Custom log history 1" /Users/YOUR_USERNAME/Library/Logs/Growl.log
{{< / highlight >}}

Unfortunately the way the log is ugly. So I said tried to make it
nicer with `perl`. The point is, I don't know `perl`. I had to google
a lot, but I'm satisfied with the resulting [growl.pl](https://github.com/gglanzani/Perl/raw/master/growl.pl).

In order to use it, save it somewhere, let's say
`~/Dropbox/Scripts/growl.pl`. Then from a terminal type

{{< highlight bash >}}
touch ~/Library/Logs/growl_geek.log
chmod +x ~/Dropbox/Scripts/growl.pl
{{< / highlight >}}

Then you're done. You may even wish to have GeekTool display the
prettified `growl_geek.log`. I do with the command
`~/Dropbox/Scripts/growl.pl && tail -20l
~/Library/Logs/secondgrowl.log`, where 20 is the number of line from
the end of the log you wish to display.
