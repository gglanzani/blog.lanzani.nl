---
title: MapToPoster
author: Giovanni Lanzani
date: 2026-01-22T00:00:00Z
url: /2026/maptoposter
snippet: |
  I just found out this beautiful utility to transform cities into beautiful posters.
link: https://github.com/originalankur/maptoposter
link_title: Transform your favorite cities into beautiful, minimalist designs.
---

I stumbled upon this beautiful utility to create minimalist posters of the (water)ways of a city.

If you have `uv`, getting started is really easy:

```
git clone https://github.com/originalankur/maptoposter  
uv venv --python 3.14.2
source .venv/bin/activate
uv pip install -r requirements.txt
python create_map_poster.py -c "Padua" -C "Italy" -t forest -d 4000
```

The last command will take a bit of time the first time, as it needs to download quite some data, but the results are beautiful:

![A MapToPoster rendering of Padua, Italy](images/2026/padua-maptoposter.webp "A MapToPoster rendering of Padua, Italy")
