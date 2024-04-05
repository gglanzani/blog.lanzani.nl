---
date: 2024-04-05T00:00:00Z
title: Docker_isbg
tags:
- docker
- github
- opensource
- lua
- mail
snippet: |
  I recently started using docker_isbg to filter spam from a remote IMAP server. 
  However, while I configured to not use gmail-specific settings, the program was
  ignoring my instructions. I fixed it by opening my first lua-based PR.
url: /2024/docker_isbg
---

I recently started using [docker_isbg], a container bundling [isbg] and [imapfilter] to filter out spam from a remote IMAP server.

Getting started is relatively straightforward and relies mostly on a JSON configuration file such as 

{{< highlight json >}}
{
  "server": "mail.somewhere.com",
  "username": "somebody@somewhere.com",
  "password": "Password",
  "isGmail": "no",                        //Optional; Default = no
  "spamSubject": "[SPAM?]",               //Optional;
  "report": "yes",                        //Optional; Default = no
  "spamLifetime": 30,                     //Optional;
  "folders": {
    "spam": "Spam",
    "ham": "ham",                         //Optional;
    "sent": "Sent",                       //Optional;
    "inbox": "INBOX"
  }
}
{{< /highlight >}}

After setting it up to run on a *non-Gmail* email host, however, I noticed the logs were complaining that `isbg` couldn't find Gmail-specific folders. That meant that, somehow, my configuration was telling `isbg` that I was on a gmail host, even though the line`"isGmail": "no"` was in my config.

After some looking around, I saw the offending piece of [code] in docker_isbg (newlines added for clarity):

{{< highlight lua >}}
if( confLoader.tableHasKey( config, "isGmail" ) 
    and config.isGmail ) 
    then 
      gmailOption = " --gmail" 
    else 
      gmailOption = "" end 
{{< /highlight >}}

The code checks whether `config` (which is a dictionary-like object resulting from the parsing of the above JSON) has a `isGmail` key, and whether it is set to anything: it can be `yes`, `no` or any other string and it will pass the `--gmail` option to `isbg`.

To fix it, I opened a [PR] that changes the above lines to

{{< highlight lua >}}
if( confLoader.tableHasKey( config, "isGmail" ) 
    and config.isGmail == "yes" ) 
    then 
      gmailOption = " --gmail" 
    else 
      gmailOption = "" end 
{{< /highlight >}}

While the project seems to be mostly abandoned so I don't expect a prompt merge, there's another easy fix: changing the `"isGmail": "no",` line to `"isGmail": false` in the configuration file as in that case the `config.isGmail` code will evaluate to `false`.

[docker_isbg]: https://github.com/DumpName/docker_isbg
[isbg]: https://gitlab.com/isbg/isbg
[imapfilter]: https://github.com/lefcha/imapfilter
[code]: https://github.com/DumpName/docker_isbg/blob/main/imapfilterExec/spamFilter.lua#L17C1-L18C1
[PR]: https://github.com/DumpName/docker_isbg/pull/10
