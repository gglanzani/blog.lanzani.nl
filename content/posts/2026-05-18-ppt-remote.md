---
title: PPT Remote
author: Giovanni Lanzani
date: 2026-05-18T18:00:00Z
url: /2026/ppt-remote
tags:
   - macOS
   - open-source
   - swift
---

I always wanted a remote for PowerPoint that would allow me to use the phone as the laser pointer. So, I've made one! (macOS only, sorry)

The app serves a control page through WebSockets, that can be accessed through a phone. There's a handy QR code to make it easy to reach the page (since it runs an open web server, there's a token at the end to make it hard to enter if you don't have it).

![](images/2026/ppt-remote-in-action.webp "The whole UI on the Mac lives in the menu bar")

The control page gives you:

- Slide navigation.
- A laser pointer toggle.
- A touch pad that moves the Mac cursor (useful for the laser pointer).

![](images/2026/ppt-remote-phone-ui.webp "The UI on the phone")

The app is open source, and you can find it on [GitHub](https://github.com/gglanzani/ppt-remote).
