---
date: 2009-07-30T00:00:00Z
title: Web of Science, Papers and Leiden University
url: /2009/WOS-and-Papers/
---

To use Leiden ezproxy with Leiden university you have to do the following. Open with Textedit.app the file
{{< highlight bash >}}
~/Library/Application
Support/Papers/PlugIns/SearchEngines/WOSSearchEngine.searchengine/Contents/Resources/gatewayurl.txt
{{< / highlight >}}
replace the address there with the following
{{< highlight bash >}}
http://wok-ws.isiknowledge.com.ezproxy.leidenuniv.nl:2048/esti/soap/SearchRetrieve
{{< / highlight >}}
Beware: there must not be any newline at the end of the file `gatewayurl.txt` (if you editor places them automatically, change editor for a moment).
Then fire up Papers. Go to Preferences -> Sources and as *Authentican URL* use
{{< highlight bash >}}
http://wok-ws.isiknowledge.com.ezproxy.leidenuniv.nl:2048/esti/soap/SearchRetrieve
{{< / highlight >}}
Check the box *Go to this page when Papers is started*. As *Library Proxy* use
{{< highlight bash >}}
http://ezproxy.leidenuniv.nl:2048/login?url=%@
{{< / highlight >}}
Restart Papers. You should be prompted for the Leiden University
username and password. Fill them in. You should now see something that says
{{< highlight bash >}}
SearchRetrieve
Hi there, this is an AXIS service!
Perhaps there will be a form for invoking the service here...
{{< / highlight >}}






