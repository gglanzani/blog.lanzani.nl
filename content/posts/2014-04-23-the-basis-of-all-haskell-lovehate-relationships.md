---
date: 2014-04-23T00:00:00Z
title: The basis of all Haskell love-hate relationships
tags: ["code snippet", "haskell", "mathematics", "programming", "tutorial"]
url: /2014/the-basis-of-all-haskell-lovehate-relationships/
---

Depends on what you think about this snippet of code:[^1]
{{< highlight haskell >}}
let square y = y * y; limit = 100 in [(x, y, z) | y <- [1..limit], x <- [1..y], z <- [1..limit],  square x + square y == square z]
{{< / highlight >}}

[^1]: It finds all the right triangles with integer sides smaller than `limit` without duplicates. If you want the version with duplicates, just use `x <- [1..limit]`
