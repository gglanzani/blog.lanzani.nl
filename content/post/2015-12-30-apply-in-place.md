---
date: 2015-12-30T00:00:00Z
title: Apply a function in place in pandas
url: /2015/apply-in-place/
---


There is one thing that is bothering me lately in pandas, and that is the apparent inability to
avoid the [SettingWithCopy] warning when I want to update a column. What I mean is the following

{{< highlight python>}}
df.loc[:, a_field] = df[a_field].apply(a_fun)
{{< / highlight >}}

very often if not always will raise the following warning

{{< highlight python>}}
./lib/site-packages/pandas/core/frame.py:3635: SettingWithCopyWarning: 
A value is trying to be set on a copy of a slice from a DataFrame.
Try using .loc[row_indexer,col_indexer] = value instead

See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
{{< / highlight >}}

The warning is not very informative, as it seems to me I'm already using `.loc` properly. The only 
way to reliably get rid of the warning was to create my own helper function

{{< highlight python>}}
def apply_inplace(df, field, fun):
    return pd.concat([df.drop(field, axis=1), df[field].apply(fun)], axis=1) 
{{< / highlight >}}

No more warnings now!

{{< highlight python>}}
df = apply_inplace(df, a_field, a_fun)
{{< / highlight >}}


[SettingWithCopy]: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
