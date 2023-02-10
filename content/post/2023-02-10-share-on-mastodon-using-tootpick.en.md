---
title: Share on Mastodon using Tootpick
author: ''
date: '2023-02-10'
slug: share-on-mastodon-using-tootpick
categories: []
tags:
  - Mastodon
image:
  caption: ''
  focal_point: ''
---

In addition to other social media sites I wanted to enable sharing on [Mastodon](https://joinmastodon.org) as well. The main difficulty is that as part of the Fediverse there is no central server that users need to access. Instead people need to sent to their instance. There are multiple solutions for that problem, but in the ned I settled on [Tootpick](https://github.com/Juerd/tootpick). I like the privacy design and the ease with which this can be self hosted. The latter should allow me to integrate it better into this sites design.

This site uses [blogdown](https://pkgs.rstudio.com/blogdown/) with the [Hugo Academic](https://academic-demo.netlify.app/) theme. I took the following steps to enable share on Mastodon:

1. Download https://raw.githubusercontent.com/Juerd/tootpick/main/index.html and save it as `static/tootpick.html`.

2. Copy `themes/hugo-academic/layouts/partials/share.html` to `layouts/partials/share.html` and add:

```html
    <li>
      <a class="mastodon"
         href="/tootpick.html#text={{ .Title | html }}%0A{{ .Permalink | html }}"
         target="_blank" rel="noopener">
        <i class="fab fa-mastodon"></i>
      </a>
    </li>
```    
