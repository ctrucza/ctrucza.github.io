---
layout: post
title:  "Roslyn Baby Steps: Getting Started"
date:   2014-08-19 12:15:41
categories: 
---

You will need Visual Studio (2013, 2014 CTP 4 as of now).

VS 2013 and VS 14 will need different downloads. Based on [a discussion on the forums](https://roslyn.codeplex.com/discussions/547581) I choose to go with the VS 14 CTP, as the VS 2013 preview will not be updated.

I will try to see how the code works in VS 2013 but the development will be primarily on VS 14 CTP. Living on the edge, eh?

to get the bits: `Install-Package Microsoft.CodeAnalysis -Pre` in the Package Manager Console

---
Next baby-steps:

- [Workspaces](roslyn-baby-steps-workspaces)
- [Syntax Analysis](roslyn-baby-steps-syntax-analysis)
- [Semantic Analysis](roslyn-baby-steps-semantic-analysis)

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