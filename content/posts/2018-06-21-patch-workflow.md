---
date: 2018-06-21T00:00:00Z
title: Git patch workflow
tags: ["git", "patch", "programming", "technology", "tutorial", "version control", "workflow"]
url: /2018/patch
---

This is totally a note for my future self.

Sometimes when working with `git` I find myself having to create a patch, because I had to merge
in my feature branch more than once, but I want to have a single commit when doing a PR.

Assuming I want to merge against `master`, and my branch is called `feature`, I can do the following

```
git checkout feature
git merge master
git diff master..feature > patch.diff
git checkout master  # the new branch should stem from master
git checkout -b feature-patch  # need a different name
git apply --ignore-space-change --ignore-whitespace patch.diff
git add .  # assuming it's a clear working directory, besides the branch
git commit -m "Add reassuring commit message here"
git push -f origin feature-patch:feature  # this will push feature-patch on the feature branch
```

It seems involved, but once you get the hang of it, it's pretty fast.
