---
date: 2009-08-24T00:00:00Z
title: Add Copy as PDF Shortcut in Mathematica
tags: ["mac", "mathematica", "pdf", "programming", "shortcut", "tutorial"]
url: /2009/Copy-as-PDF-Mathematica/
---

To add a shortcut for the Menu Item Copy as PDF you have to do the following (with a Mac) go to
{{< highlight bash >}}
/Applications/Mathematica/SystemFiles/FrontEnd/TextResources/Macintosh
{{< / highlight >}}
and edit the file
{{< highlight bash >}}
MenuSetup.tr
{{< / highlight >}}
changing the line
{{< highlight bash >}}
MenuItem["PDF", FrontEnd`CopySpecial["PDF"]],
{{< / highlight >}}
to
{{< highlight bash >}}
MenuItem["PDF", FrontEnd`CopySpecial["PDF"], MenuKey["C", Modifiers->{"Command", "Option"}]],
{{< / highlight >}}
In this way Command + Option + C will copy the object as a PDF in the clipboard.


