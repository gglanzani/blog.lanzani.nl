---
date: 2024-02-06T00:00:00Z
title: Decode your URLs
tags: ["javascript", "microsoft", "productivity tools", "programming", "safelinks", "typinator", "url decoding"]
url: /2024/decode-your-urls
---

Today, I read Terence Eden's [article] about the abundance of Microsoft's [safelinks] systems. One example he gives is

> https://eur03.safelinks.protection.outlook.com/?url=https%3A%2F%2Fcfa.nhs.uk%2Fabout-nhscfa%2Fcorporate-publications%2FSIA-23%2FSIA-2023-foreword&data=05%7C01%7CAnnualReport%40dhsc.gov.uk%7C26af6f102198492b3bcd08dbca6a3cc3%7C61278c3091a84c318c1fef4de8973a1c%7C1%7C0%7C638326329851162850%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C3000%7C%7C%7C&sdata=WK%2FuqOZJ0ixZpvJIyJiCWRt5hxTwBfcwbONaUxJViBw%3D&reserved=0

As my work email is hosted by Microsoft, I'm bugged to no end by this system, which doesn't make it easy to extract the URL in a readable format (read the blog if you want to know more shortcomings of the system).

For a couple of years, I have a [Typinator] snippet that decodes the URL, so that by typing `;dec`, the following script is triggered

{{< highlight js >}}
{/JavaScript
let URI = "{clip}".split("=", 2)[1].replace("&data", "")
let encURI = decodeURIComponent(URI)

encURI
}
{{< /highlight >}}

Converting the above link to 

> https://cfa.nhs.uk/about-nhscfa/corporate-publications/SIA-23/SIA-2023-foreword

The script is not perfect, but, since defining it, Typinator analytics tell me I've used it some hundred times and I can't recall when it didn't just work.

[Typinator]: https://www.ergonis.com/typinator/
[article]: https://shkspr.mobi/blog/2024/02/safelinks-are-a-fragile-foundation-for-publishing/
[safelinks]: https://learn.microsoft.com/en-us/microsoft-365/security/office-365-security/safe-links-about?view=o365-worldwide#safe-links-settings-for-email-messages
