---
date: 2015-07-27T00:00:00Z
title: A functional count in Python
url: /2015/a-functional-count/
---

Today I was sifting through the documentation for `itertools.count`

> Return a count object whose .next() method returns consecutive values.
> Equivalent to:

{{< highlight python >}}
def count(firstval=0, step=1):
    x = firstval
    while 1:
        yield x
        x += step
{{< / highlight >}}

Since itertools is supposed to provide you with better tools to write functional Python code, I
thought it was odd that they have an example with `x += step`, redefining the `x` variable with
every new invocation (although, to their defense, they only say that it is equivalent to the
provided definition.

I sat therefore down wanting to create a "functional" version of it. Here's what I came up with
(Python 3.4 required)

{{< highlight python >}}
def _count(first, step):
    while 1:
        yield first
        yield from count(first + step, step)
{{< / highlight >}}

