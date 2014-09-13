---
layout: post
title:  "Detecting smells from commits"
date:   2014-08-19 12:15:41
categories:
---

Story
---
- several commits over a few weeks
- every commit changes the same 6 files among others
- without looking into the files you can tell that there is something fishy there (shotgun surgery)
- looking into them, you see code duplication: the same code is added/changed in all six of them
- the design is broken
- generalize: look into history and see which files change together. They might point to a smell