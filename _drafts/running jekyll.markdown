---
layout: post
title:  "Running Jekyll"
#date:   2014-08-19 12:15:41
categories: 
---

`bundle exec jekyll serve`



if main.css missing:

- `gem install jekyll-compass`

- Add the gem to your jekyll website's _config.yml file:

{% highlight c# %}
gems:
- jekyll-compass
{% endhighlight %}

(from readme of https://github.com/mscharley/jekyll-compass)


Or `bundle update` ...