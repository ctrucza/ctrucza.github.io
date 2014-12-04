---
layout: post
title:  "Roslyn Baby Steps: Syntax Analysis"
#date:   2014-08-19 12:15:41
categories:
---

Once you [get access to the documents of a project of a solution]() it's time to do some woodoo magic.

Let's review how we get to a document:

{% highlight C# %}
private const string root = @"..\..\";
const string solutionFilePath = root + @"..\Hacks.sln";

static void Main(string[] args)
{
    var workspace = MSBuildWorkspace.Create();
    var solution = workspace.OpenSolutionAsync(solutionFilePath).Result;
    foreach (var project in solution.Projects)
    {
        foreach (var document in project.Documents)
        {
            DumpDocument(document);
        }
    }
}
{% endhighlight %}

The first step in reading the code is asking for a syntax tree:

{% highlight C# %}
private static void DumpDocument(Document document)
{
    var syntaxTree = document.GetSyntaxTreeAsync().Result;
    // ...
}
{% endhighlight %}

You cand dump the whole tree on the console:

{% highlight C# %}
private static void DumpDocument(Document document)
{
    var syntaxTree = document.GetSyntaxTreeAsync().Result;
    DumpTree(syntaxTree.GetRoot());
}

private static void DumpTree(SyntaxNode node)
{
    Console.WriteLine("({0}) {1}", node.CSharpKind(), node.GetText());
    node.ChildNodes().ToList().ForEach(n => DumpTree(n));
}
{% endhighlight %}

Good luck reading it :)

The syntax visualizer
---
If you install the [Roslyn Syntax Visualizer](https://visualstudiogallery.msdn.microsoft.com/70e184da-9b3a-402f-b210-d62a898e2887) you can explore the syntax graph much easier.

Open the syntax visualizer window from the `View > Other Windows > Roslyn Syntax Visualizer` menu.

![Open the syntax visualizer window](/assets/OpenSyntaxVisualizerView.png "Open the syntax visualizer window")

It will show the syntax tree of the currently active document:

![Syntax tree](/assets/SyntaxTree.png "Syntax tree")

If you wish, you can also look at a graph of a node:

![Open the syntax graph window](/assets/OpenSyntaxGraphView.png "Open the syntax graph window")

![Syntax graph](/assets/SyntaxGraph.png "Syntax graph")

Finding stuff in the syntax tree
---

Let's try to do something more useful: find all class declarations in the solution:

{% highlight C# %}
private static void DumpDocument(Document document)
{
    var syntaxTree = document.GetSyntaxTreeAsync().Result;
    var roodNode = syntaxTree.GetRoot();
    var classDeclarations = roodNode.DescendantNodes().OfType<ClassDeclarationSyntax>();
    foreach (var classDeclaration in classDeclarations)
    {
        Console.WriteLine(classDeclaration.Identifier);
    }
}
{% endhighlight %}

Or all the methods in the solution:

{% highlight C# %}
private static void DumpDocument(Document document)
{
    var syntaxTree = document.GetSyntaxTreeAsync().Result;
    var roodNode = syntaxTree.GetRoot();
    var methodDeclarations = roodNode.DescendantNodes().OfType<MethodDeclarationSyntax>();
    foreach (var method in methodDeclarations)
    {
        Console.WriteLine(method.Identifier);
    }
}
{% endhighlight %}

You get the idea.

(TBD: syntax walkers)