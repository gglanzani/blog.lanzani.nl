---
date: 2022-05-01T07:00:00Z
title: Test your Machine Learning models in production
url: /2022/test-ml-in-prod
---

Have you ever thought why the flight attendants bother giving safety instructions? Do you listen to them?

Flight attendants are stuck. They can’t go off script.

Probably a long time ago, there were tests on how to deliver those safety instructions to passengers.

The current way was tested not with busy passengers needing to get somewhere, but people recruited for the purpose. It probably fared better than anything else.

Yet, when applied in real life, it sucks. We don’t listen to what they say.

I see the same mistake made in data science: people test their model with real data, but not in production.

I used to tell my classes a story of a big online retailer developing a much better version of their recommender — “customers who bought this, also bought that” type of thing.

With the new recommender, fewer clicks were necessary to understand the set of items the customer wanted to buy.

Before rolling out, they A/B tested it — luckily.

To their surprise, people exposed to the new version, were closing their browser more quickly **without** buying!

Some of them were logged in, so they decided to investigate.

It turns out, customers were creeped out by the eerie accuracy of the new recommender. They left the website, afraid of what else the retailer would find out about them.

The retailer went back to the old version.

It doesn’t matter how enthusiast data scientists are about the model.

Without testing in production, it counts for nothing.
