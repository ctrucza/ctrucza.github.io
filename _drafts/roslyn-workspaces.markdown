---
layout: post
title:  "Roslyn: Workspaces"
date:   2014-08-19 12:15:41
categories: Roslyn, workspace
---

Open a solution
---

{% highlight C# %}
var w = MSBuildWorkspace.Create();
var solution =w.OpenSolutionAsync(path).Result;
var project = w.OpenProjectAsync(@"").Result;
{% endhighlight %}
