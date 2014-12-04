---
layout: post
title:  "Roslyn Baby Steps: Setup"
#date:   2014-08-19 12:15:41
categories: 
---

To start playing with Roslyn, you will need Visual Studio 2013 or 2015 Preview, and you will have to install the Roslyn Nuget package.

In the package manager console:

`Install-Package Microsoft.CodeAnalysis -Pre`

This will download the needed assemblies and and them to your project references . Review them and remove the ones you don't need. As I work only in C#, I removed all VisualBasic related assemblies.

Let's give it a try:

{% highlight C# %}
using System;
using Microsoft.CodeAnalysis.CSharp;
using Microsoft.CodeAnalysis.Text;

namespace SettingUp
{
    class Program
    {
        static void Main(string[] args)
        {
            var tree = CSharpSyntaxTree.ParseText(@"
                public class Foo
                {
                    public int Bar()
                    {
                        return 42;
                    }
                }
            ");

            foreach (TextLine line in tree.GetText().Lines)
            {
                Console.WriteLine(line);
            }
        }
    }
}
{% endhighlight %}

If you are greeted with an exception

`Unhandled Exception: System.IO.FileNotFoundException: Could not load file or assembly 'System.Composition.TypedParts, Version=1.0.27.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a'`

or similar, [you need to install another package](http://stackoverflow.com/questions/25658599/where-is-the-system-composition-typedparts-dll):

`Install-Package Microsoft.Composition`

---
Next baby-steps:

- [Workspaces, Solutions and Projects](roslyn-baby-steps-workspaces-solutions-and-projects.html)
- [Syntax Analysis](roslyn-baby-steps-syntax-analysis.html)
- [Semantic Analysis](roslyn-baby-steps-semantic-analysis.html)

---

Useful links:

- [Roslyn FAQ](https://roslyn.codeplex.com/wikipage?title=FAQ)
- [Official Roslyn Site](https://roslyn.codeplex.com/)
- [Roslyn Overview](https://roslyn.codeplex.com/wikipage?title=Overview)
- [Samples and Walkthroughs](https://roslyn.codeplex.com/wikipage?title=Samples%20and%20Walkthroughs)
    - [Syntax Analysis in C# (pdf)](https://www.codeplex.com/Download?ProjectName=roslyn&DownloadId=822179)
    - [Semantic Analysis in C# (pdf)](https://www.codeplex.com/Download?ProjectName=roslyn&DownloadId=822179)
- [Sources](http://source.roslyn.codeplex.com/)
    - [Building, Testing and Debugging](https://roslyn.codeplex.com/wikipage?title=Building%2c%20Testing%20and%20Debugging)