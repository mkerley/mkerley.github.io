---
layout: post
title: C++ Renaissance? I'm not so sure.
date: "2011-10-18 18:00:02 -0700"
categories:
  - Software Development
tags:
  - c++
  - microsoft
  - metro
excerpt_separator: <!--more-->
redirect_from: "/wp/2011/10/18/c-renaissance-im-not-so-sure/"
---

<aside>
  <figure>
     <img src="/assets/images/2011/10/leo_cplusplus-221x300.jpg" alt="Leonardo da Vinci" />

     <figcaption>Leonardo da Vinci. Not to be confused with Leonardo da Turtle.</figcaption>

  </figure>
</aside>

I've seen [various](http://drdobbs.com/cpp/231901048) [posts](http://drdobbs.com/cpp/231900562) lately talking about a "C++ Renaissance". I'm trying to make some sense of this, and mainly to figure out whether it's true.

<!--more-->

It seems there is some belief that just because there is a new C++ standard ([C++11](http://en.wikipedia.org/wiki/C%2B%2B11), formerly known as C++0x) there is a "renaissance". Most of the posts I've seen on the subject are basically restatements and requotes from a recent [interview](http://blogs.msdn.com/b/vcblog/archive/2011/02/08/10126533.aspx) with a couple guys on Microsoft's Visual C++ team. So far all I can tell is that the VC++ guys are hyping their own product, and some people are buying into it.

I haven't heard of any widespread transition from Java/PHP/Python/Ruby/Perl/Objective-C/etc to C++. Server-side developers who are developing for maximum performance may use C++, but they probably already were before the so-called renaissance. Mobile developers are still primarily using Objective-C on iOS and Java on Android. Console game developers were already using C++. Embedded developers will continue (not start) to use C++ if they have the luxury; otherwise they'll use C or assembler. Kernel developers will continue to use C.

The major remaining category is desktop apps. Mac apps are generally written in Objective-C (in fact that's the only way to access the Cocoa GUI framework). Linux code is largely written in C. So the renaissance must be on Windows, right? Well, Microsoft seems to be the focus of this "renaissance", and even they aren't fully embracing C++. Yes, it will be possible to write Windows desktop apps in C++, but [Metro](http://www.zdnet.com/blog/microsoft/microsoft-to-developers-metro-is-your-future/10611) is Microsoft's latest-and-greatest development tool, and Metro apps are mostly about HTML and Javascript. And .NET (primarily C#, VB.NET) probably still fits into Microsoft's plans somewhere.

So where exactly is the renaissance supposed to happen?
