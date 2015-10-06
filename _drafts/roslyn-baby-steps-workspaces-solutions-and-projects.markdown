---
layout: post
title:  "Roslyn Baby Steps: Workspaces, Solutions, Projects and Documents"
#date:   2014-08-19 12:15:41
categories:
---

As we saw in the [previous post](), parsing any C# code is trivial. Playing with Roslyn can be done with this approach. the samples in the [Roslyn examples]() use this method.

To analyze individual files you could write

{% highlight C# %}
var code = new StreamReader("YourFile.cs").ReadToEnd();
var tree = CSharpSyntaxTree.ParseText(code);
DumpTree(tree);
{% endhighlight %}

(to see a complete example on parsing individual files, [see this blog post](http://blog.filipekberg.se/2011/10/20/using-roslyn-to-parse-c-code-files/))

Doing any kind of useful processing though is not gonna fly this way. You might want to load solutions and projects.

Welcome to workspaces.

In order to open a solution, we need to create a workspace:

{% highlight C# %}
var workspace = MSBuildWorkspace.Create();
var solution = workspace.OpenSolutionAsync(path).Result;
{% endhighlight %}

After this you can iterate on the projects from the solution:

{% highlight C# %}
foreach (var project in solution.Projects)
{
    DumpProject(project);
}
{% endhighlight %}

If you want to open a project, you can write:

{% highlight C# %}
Project project = workspace.OpenProjectAsync(projectFilePath).Result;
DumpProject(project);
{% endhighlight %}

Once you have a reference to a project you can get the source files:

{% highlight C# %}
foreach (var document in project.Documents)
{
    var tree = document.GetSyntaxTreeAsync().Result;
    DumpTree(tree);
}
{% endhighlight %}

And finally the `Document` gives you access to all the Roslyn goodies. For example to get all the classes in a file you would write this:

{% highlight C# %}
var root = document.GetSyntaxRootAsync().Result;
var classes = root.DescendantNodes().OfType<ClassDeclarationSyntax>();
foreach(var class in classes)
{
    Console.WriteLine(class.Identifier);
}
{% endhighlight %}

And to dump a syntax tree:

{% highlight C# %}
private static void DumpTree(SyntaxTree tree)
{
    foreach (TextLine line in tree.GetText().Lines)
    {
        Console.WriteLine(line);
    }
}
{% endhighlight %}
