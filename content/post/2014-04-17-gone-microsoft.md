---
date: 2014-04-17T00:00:00Z
title: Gone Microsoft
url: /2014/gone-microsoft/
---

My first machine was some IBM 286, which a neighbour gave to my father when I was a kid. At work
they were upgrading to, probably, 386's so he could take it home. Since he already had one, we were
the lucky ones. I think I was six, so this was probably 1990.

The machine ran DOS and some years passed by before we got got something that could run a GUI. But
when the GUI finally came, it was a revolution for us. We began with Windows 95. As the years
passed, Windows would get better and better; however of those years, I vividly remember one thing:
all the time lost trying to troubleshoot and fix that crap. When, probably in 2003, I got an iPod I
was surprised that this, small, highly technological music player would just... work. A friend at my
university had a Powermac at home and he told me that Macs were all like that: they just worked. So
I got a part time job and by next spring (the new Intel Macbook Pro's were already out), I
bought an iBook G4. It was a fantastic machine and I loved every inch of it (12, in diagonal). From
the battery life to the trackpad to the integration between hardware and software. That day I said
to myself that I would never go back to Microsoft products and PC's in my life.  At the Physics
institute we were using [LaTeX] and all kind of scientific software anyway, so Office was never an
issue.

In the years that ensued, I never looked back to that decision. Fed up with "Thanksgiving customer
support", or whatever it's called here in Europe, I had all my relatives switch to Macs: my
parents, my brother, my uncle, my in-laws (parents and brothers) and all the friends that ever
asked me to help them with their Microsoft products. I lost count with the years, but up to before
getting married I convinced some 25 persons to switch.

Fast forward 10 years later. I found myself using:[^using]

+ Google Apps for this domain;
+ Google Apps at [work], with Google Drive to sync work's files;
+ Dropbox to sync my personal files, used as a back-end of a handful of (iOS) apps;
+ iWork for my Office needs (yeah, I'm not in academia anymore).

Something in these setup began to crack though. Google "coolness" began to fade. First they retired
the mobile Exchange support for Gmail and then they sun setted their Google Apps free tier.[^1] I
was grandfathered in their new plan, but I felt like an unwanted guest.[^2] At the same time, or
around that time, Google initiated a [Google plus-ification][plus] of their product line to push
everybody onto their Google Plus wagon. They also initiated a Gmail redesign that resulted in worse
UI and UX.[^3] The list doesn't end here. Google sun setted one of my favorite service, Google
Reader, effectively reducing the usefulness of a Google account.

At the same time Microsoft completely revamped their web mail offering, [Outlook.com][outlook].
They offer custom domain (up to 50 users), unlimited space, mobile Exchange and IMAP access, and
redesigned their website in what I would consider a good way (although I still don't use it).

So one weekend I switched my email from Google to Microsoft. It felt strange at the beginning, but
I really enjoyed having mobile Exchange back and unlimited space.

Some months passed and Microsoft kept cooking what were, hear hear, actually really cool products.
They announced a new, free to use, version of [OneNote].  They touted how awesome that was, its
cross platform availability (I use a Mac an iPhone and an iPad, so being able to access all my notes
from all my devices was kind of a big deal), and the ability to sync, for free, up to 7GB of notes
through OneDrive.

I decided to give it a spin because, as a geek, I'm always searching for the ultimate productivity
tool and even in academia OneNote was known to be a great piece of software.  What I found is a
well behaving and native Mac application, self-contained (in fact downloadable from the App Store)
and overall nice to use. The iPad version is equally nice and the syncing and collaboration[^4]
capabilities are also impressive.

Next in line, can you guess? One of the hottest startup in the Valley that welcomed Dr. Rice on the
board? Up to a few months ago, it would have been impossible for me to abandon Dropbox, because so
many apps I use rely on it as their sync/storage back end. This included [1Password], [ScannerPro]
and [Nebulous Notes][Nebulous], notably. But, recently, my 1Password apps freed themselves from
Dropbox because I want to sync my Agile Keychain with my wife's Macbook (through iCloud now). As
for Nebulous Notes: I used it only to jot down quick notes, so OneNote was the ideal replacement
for that.

That left ScannerPro: for those of you not familiar with it, ScannerPro is an amazing app that can
upload scanned documents to Dropbox, Evernote, Google Drive and WebDav.  I could have connected
it to my company's Google Drive but if you ever used Google Drive on the Mac, you know how much it
sucks. I don't know if it is specific to my setup, but once a day I would get the dreaded
[Google Drive needs to quit][GDrive] window that forced me to, well, quit it. As a result, I only
use Google Drive through the web interface thus the GDrive route for ScannerPro was not completely
satisfactory.

But some days ago the fine folks at [doo] just published [Scanbot], an app similar to ScannerPro
with the notable difference that it syncs to, among others OneDrive! Here's my Dropbox replacement!
And by using OneDrive for my files, every Office file on my OneDrive folder, can be opened, for
free, by the Office web apps!

And with that Dropbox went to the Trash and another Microsoft app found its home on my Application
folder.

After all of this happened, I asked myself why I gave "up" so easily on my existing setup. I don't
know the answer, but a couple of ideas comes to mind:

+ the first reason can be probably ascribed to [Justin] Williams, of [Second Gear][secondgear]
  fame; I don't know exactly how it went, but probably after his acquisition of [Glassboard], whose
  back end runs on [Azure], he started tweeting and blogging about Microsoft.  And he painted quite
  a different picture from the Microsoft I knew, in a positive way. He let me take a peak from a
  new angle;
+ the second reason is that Microsoft has radically changed from the Microsoft I knew. At their
  latest developer conference, [Build], they announced things which were imaginable 10 years ago:
  not only they open sourced a bunch of .NET components and related technologies, but they also
  showed on stage iPhones, iPads, were the host of [The Talk Show][Talk] by John Gruber[^5] and
  stopped with the notion that everything and everybody should run Windows for Microsoft to be
  happy.

That said: not everything is perfect in Microsoft land. Here's a short list of what I don't like:

+ Outlook.com filters[^6] and keyboard shortcuts sucks!
+ Adding email aliases to Outlook.com has to be done via an obscure command line app that only
  works on Windows (I had to download a Windows VM to make it work) and you're limited to 5. Here
  Gmail is years ahead;
+ OneDrive does not sync back files very easily; when I modify a file via an Office web app, it
  takes a while to get it back to my Mac;
+ OneNote for Mac still lags severely behind its Windows counterpart and the iPhone apps is behind
  the iPad app.

Considered that I cannot deny that the new Microsoft is a welcome change for me; they have
incredibly talented people and benefiting from their talent without having to use Windows PCs (or
tablets) is a huge win for Apple users.


[^using]: This is not an exhaustive list of course.
[^1]: This is not completely accurate: there still is some web page through which you can get a plan that supports only one account.
[^2]: You may argue that paying could have solved it, but that's another story. I certainly would if I'd make money off this site.
[^3]: Although I don't use the web interface for my mail. There's an [app] for that.
[^4]: I had a colleague download it and we immediately created a *Knowledge Base* of tips & tricks and tutorials for some of the software/technologies that we use.
[^5]: If you don't know who John Gruber is, let me tell you: it's a big deal!
[^6]: Only one condition per filter? Really?

[LaTeX]: http://www.latex-project.org
[work]: http://godatadriven.com
[plus]: http://www.techopedia.com/definition/28281/googleification-googleification
[stringer]: http://github.com/swanson/stringer
[app]: http://freron.com
[outlook]: http://www.outlook.com
[OneNote]: http://blogs.office.com/2014/onenote-now-on-mac-free-everywhere-and-service-powered/
[1Password]: http://agilebits.com/1password
[Justin]: http://twitter.com/justin
[ScannerPro]: http://readdle.com/products/scannerpro/
[Nebulous]: http://nebulousapps.net
[Gdrive]: https://duckduckgo.com/?q=google+drive+needs+to+quit
[doo]: https://doo.net
[Scanbot]: http://scanbot.io/en/
[secondgear]: http://www.secondgearsoftware.com
[Glassboard]: http://www.glassboard.com
[Azure]: http://azure.microsoft.com/en-us/
[Build]: http://www.buildwindows.com
[Talk]: http://www.muleradio.net/thetalkshow/
