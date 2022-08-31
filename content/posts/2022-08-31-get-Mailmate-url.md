---
date: 2022-08-31T00:00:00Z
title: Get URL of selected Mailmate messages
url: /2022/get-mailmate-url/
---

One of the features of my favourite email client, [Mailmate], is the ability to copy a unique link per message.

Upon clicking the link from anywhere, Mailmate will be opened showing the associated email.

I use this feature a lot to link to-do in [Things]. Mailmate even offers a bundle to automatically add an email link to Things.

However sometimes I already have an existing to-do, and I need to paste the Mailmate link.

To make it easy, I created an AppleScript in [Typinator] that pastes the link of the selected Mailmate message.

Reported here for posterity :)

{{< highlight applescript >}}
tell application "MailMate"
	set selectedMessages to messages
	set theMessage to item 1 of selectedMessages
	return message url of theMessage
end tell
{{< / highlight >}}


[Mailmate]: https://freron.com
[Things]: https://culturedcode.com/things/
[Typinator]: https://www.ergonis.com/products/typinator/index.html
