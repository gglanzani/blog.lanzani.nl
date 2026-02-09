---
title: Couple Mailmate to Things for Maximum Productivity
author: Giovanni Lanzani
date: 2026-02-09T00:00:00Z
tags: ["Keyboard Maestro",  "Mailmate", "Things", "GTD", "automation"]
url: /2026/couple-mailmate-to-things-for-maximum-productivity
---

Recently, [Michael Lopp] wrote [I Hate Fish] in which he outlined his approach to getting work done.

The simplicity to remember when to do what resembles how I use [Things] (which he also uses) in combination with [Mailmate] and [Keyboard Maestro]:

- When an email requires follow-up, I hit `⌃ ⌥ ⇧ ⌘ R` and I type in how many days I want to be reminded.
![](images/2026/remind-me-in-7.webp "Keyboard Maestro asking me in how many days I should be reminded")
- The subject is then copied and becomes the title of the item. A clickable link to the email is added as a note.
![](images/2026/Things3-remind-me-in-7.webp "The item in Things")
- That's it. On the day I have to follow-up, the item will pop up in my Things list.

The system also works when I just replied to an email, and I want to be reminded if there has been a follow-up or not.

The core of the system relies on this Keyboard Maestro macro
![](images/2026/keyboard-maestro-remind-me-in-7.webp "The Keyboard Maestro macro")

All the heavy lifting is done by this bit of JS:

```js {linenos=true}
let addDays = (days) => {
    let d = new Date();
    d.setDate(d.getDate() + days);
    return d.toISOString().split('T')[0];
};

function createThingsTodo(name, activationDate, notes) {
    let url = "things:///add?";
    let params = [];
    
    if (name) params.push("title=" + encodeURIComponent(String(name)));
    if (activationDate) params.push("when=" + String(activationDate));
    if (notes) params.push("notes=" + encodeURIComponent(String(notes)));
    url += params.join("&");
    
    let app = Application.currentApplication();
    app.includeStandardAdditions = true;
    app.openLocation(url);
}

let d = parseInt(kmvar.Days, 10);

let mailmate = Application("Mailmate");

let message = mailmate.messages[0];
let name = message.name();
let URL = message.messageUrl();
// Usage examples
createThingsTodo(name, addDays(d), URL);
```

Some notes:

- Line 7: I didn't use the Things [JXA] API but its [URL scheme] as `activationDate` (line 12) is somehow not supported there 🤪.
- Line 21: This is how you fetch the `Days` variable from the modal.
- Line 25: It's important to invoke the script with a message selected.

You can download the macro [here].

[Michael Lopp]: https://randsinrepose.com/
[I Hate Fish]: https://randsinrepose.com/archives/i-hate-fish/
[Slack]: https://randsinrepose.com/welcome-to-rands-leadership-slack/
[Things]: https://culturedcode.com/things/
[Mailmate]: https://freron.com
[Keyboard Maestro]: (https://www.keyboardmaestro.com/main/)
[JXA]: https://github.com/JXA-Cookbook/JXA-Cookbook
[URL scheme]: https://culturedcode.com/things/support/articles/2803573/
[here]: /remind-me-in-7.kmmacros
