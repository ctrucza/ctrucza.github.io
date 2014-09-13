---
layout: post
title:  "Roslyn Baby Steps: Workspaces"
date:   2014-08-19 12:15:41
categories:
---

Open a solution
---

{% highlight C# %}
var workspace = MSBuildWorkspace.Create();
var solution = workspace.OpenSolutionAsync(path).Result;
solution.Projects.ForEach()
var project = w.OpenProjectAsync(@"").Result;
{% endhighlight %}
