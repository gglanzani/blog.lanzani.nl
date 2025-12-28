---
date: 2011-09-30T00:00:00Z
title: Stop reading HN, start reading HN newsletter
tags: ["hacker news", "newsletter", "productivity", "ruby", "sinatra", "technology review", "web apps"]
url: /2011/interview-kd/
---

You have to admit that, even though most of the news point and can be
found elsewhere, [Hacker News](http://news.ycombinator.com) (HN from now
on) is a pretty amazing place. It has a thriving and highly-educated
community of hackers reading, writing and posting every day. Heck,
there's even a book with the best comment of one of them,
[edw519](http://edweissman.com/i-turned-my-hacker-news-comments-into-an-eboo). 

However, if you get distracted easily, you
end up downloading 33GB of [academic
papers](http://thepiratebay.org/torrent/6554331/Papers_from_Philosophical_Transactions_of_the_Royal_Society__fro)
and maybe reading some of them. Or you get caught in a [HTML5 version of
Mario](http://mario.fromlifetodeath.com/) and there goes your day. 

Many people took the wrong approach to that, and created [HN for your
workspace](http://www.catonmat.net/blog/follow-hacker-news-from-the-console/),
which may get you out of troubles with your boss or with your wife,
not solving the real problem.

One HN reader, [duck](http://news.ycombinator.com/user?id=duck), once
said

> there are times when I can't get on for a week or when I know if I
> open HN I will end up on there for an hour or more.

Well, it turns out that **duck** is a real hacker, and he puts his money
where is mouth is. He's name is Kale Davis and he builds *web apps and created
the weekly [Hacker Newsletter](http://www.hackernewsletter.com/),* as
his website says. Yeah, you read it right, he made what he needed, and
made it available for everyone.

Not only that, but Kale has also agreed to reveal what lies behind the
scene of the newsletter.  

**Giovanni** Kale, thanks for your time. When you [told
HN](http://news.ycombinator.com/item?id=1494362) you were going to build
a newsletter, you said it would contain the *top items on Hacker News*,
an *editorial side*, the *best "Ask HN" posts, job threads, and a look
back at some past articles that would still be useful.* I'd say that so
far it is really working out great. 

But with all the weekly posts, how do you handpick the best? Do you ever
feel you let something out?

**Kale** Thanks Giovanni for asking me to do an interview! Yeah, it has
gone very well. I didn't imagine that one year later I would have 4000+
subscribers, so I'm very happy with how it has gone. As far as picking
posts for each issue, I scrape HN every 30 minutes and then use a custom
Ruby/Sinatra web app to build out the newsletter. I use a lot of regex
statements and look at other metrics like votes, number of comments, and
who commented on it. I also use what I up-voted during the week as a
gauge too.

I definitely miss some every week, there is just too many to include. I
struggle with keeping the links down to a manageable amount for each
section too, so sometimes I might include one and then have to remove it
before I publish the newsletter. I often
[tweet](http://twitter.com/kale) or use [Google
Plus](https://plus.google.com/101679672756769936865) to share links that
I missed or didn't make the final cut.

**G.** So you really are an hacker! In your [about
page](http://www.kaledavis.com/about.html) you wrote that you *come from
the world of Microsoft*. What do you mean by that? And how do you became
a "hacker" who *enjoy Ruby hacking the most*?

**K.** Haha, yeah, one of these days I'll actually finish that about
page. When I say that, I don't mean that I have worked for Microsoft,
but rather for the last 13 years I've been doing nothing but either
operations or development work with Microsoft products, mainly within
the corporate IT space. Not the most exciting jobs, but nevertheless
they have been good and I've learned a lot.

I got into Ruby right around when Ruby on Rails came out, I think late
2005. I've always been into scripting languages like Perl and PHP to
create simple sites for myself or friends, and actually used Perl a lot
within the Windows environment to do sysadm tasks.  Basically, if a
task was repeatable, I would write a Perl script to do it. The moment
I saw Ruby in action though I knew it was a huge leap from what I had
been doing, so I jumped on it and been using it ever since and rewrote
everything I still used that was in Perl. Currently, I do some freelance
work on the side using it and at some point I would like to get into a
position doing Ruby or Rails programming full-time.

**G.** Ok, back to the newsletter then. Using again your words:

> I want to do some original content as well and have some different
> ideas that I am working on, but one will be highlighting projects that
> HN members have release (both small and large).

Right now you highlight HN members' project in the **Code** section in
the newsletter. What other ideas you had at the time? Should we expect
something new in the near future?  

**K.** From the beginning I wanted to make HNL more than just a
newsletter and wanted it to have that community feel just like HN has.
I've
[done](http://us1.campaign-archive.com/?u=faa8eb4ef3a111cef92c4f3d4&id=c056a2d6d8)
[some](http://us1.campaign-archive.com/?u=faa8eb4ef3a111cef92c4f3d4&id=2342296d64)
[interviews](http://us1.campaign-archive.com/?u=faa8eb4ef3a111cef92c4f3d4&id=37defb68e2)
in past and things like getting everyone to share what they are
[reading](http://www.kaledavis.com/2011/hnl-book-list-vol1.html).
In an upcoming issue look for my next idea which involves a spin on the
great [The Setup](http://usesthis.com/) blog and I have a few founder
interviews lined up too.

**G.** What kind of feedback do you receive in general for these ideas
and the newsletter?  What's the funniest email you got? Did someone ever
offered you a job because you run the thing? 

**K.** I get a lot of great feedback. With every issue I get anywhere
from a couple to a dozen replies back with feedback. Most of the time it
is just subscribers thanking me, but other times it is someone offering
feedback on how to make the newsletter better or other ideas. The
funniest one I have received was from a passionate guy that couldn't
understand why I didn't link to one of
[patio11](http://news.ycombinator.com/user?id=patio11)'s great articles.
He was quite animated and sad that I had missed it! The funniest part
was I couldn't believe I missed it either (turned out to be a bug in my
scraper). Regarding jobs, I have been offered a job (which I turned
down), but even better than that is I have made lots of great contacts.

**G.** We all know that great feedback is essential to keep things
going, because money ain't everything. However I guess that money is
what you need to keep the newsletter going. That is probably the reason
that made you insert sponsors in the newsletter. Nowadays more and more
businesses are using this form of advertisement, unobtrusive and
respectful for the reader. How do you pick your sponsor and is
this model of revenue, as opposed to wild CPM ads, working out for you?

**Kale** Since I started Hacker Newsletter as a side project, making
money was never really something I thought about. However, as the list
grew, my costs went up and so did the time commitment. Around the same
time people started asking about sponsoring the newsletter, so I knew
I could start doing some advertising and in the end I think it is a
win win for both myself and the subscribers. I'm glad I waited until
about a year into it though just so I didn't let that distract from
the original goal and I think letting it happen organically made the
transition really easy. I do handpick them and recently hooked up with
[LaunchBit](http://www.launchbit.com/) to help with the process. It
is working out great!

**G.** I recently made the switch from Emacs to Macvim, because of the
better Cocoa compatibility and because I was a bit tired of the [Escape
Meta Alt Control
Shift](http://www.emacswiki.org/emacs/EscapeMetaAltControlShift)
madness. I miss some features, but I appreciate many things. I heard you
use Vim as well. Can you tell us why the newsletter would have not be
possible were you using Emacs?

**Kale** Haha, I do use Vim and glad to hear you made the switch. It
would probably be pretty funny watching me try to use Emacs, but since
I use my custom app to build out each issue, I hardly ever have to open
up a text editor in the creation process so it wouldn't be too big of
a deal now. However, I'm constantly working on my code and adding new
features, so I guess that would come to a stop until I picked up Emacs. 
:)

Thank you Kale!
