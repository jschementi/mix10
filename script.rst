===================================================
Pumping Iron (on the web) - IronRuby and IronPython
===================================================
by Jimmy Schementi

--------
Abstract
--------
Come learn how to use IronRuby and IronPython on the web, be it on the server
with Microsoft ASP.NET MVC to simply the writing of your controllers and
views, or on the client with Microsoft Silverlight - letting you write
Silverlight apps as easily as HTML apps. But you don't have to start from
scratch to use these languages today; they support using their engines from
static Microsoft .NET languages (C# and Visual Basic .NET, for example),
enabling you to script your existing applications and use libraries written in
other languages. Adding scripting to your web applications lets you get things
done, like writing ASP.NET MVC views in embedded Ruby (ERb), or testing you
web applications; a low-risk way to introduce dynamic languages to the rest of
your team, and make writing tests fun too! Lastly, we discuss the roadmap and
state of these languages.


-----
ACT I
-----
*Set up the story*

1. Setting (where are we, and when is it)

   Simple programming models are increasingly popular for building web applications

2. Protagonist (who are we in this setting)

   Web developers on the .NET platform who want to get things done

3. Imbalance (why are we here)

   Web developers move to other platforms for simpler programming models, rather than using the "enterprizy" tools

4. Balance (what do we want to do see happen)

   Simplify .NET web development and make it relevant to a broader audience

5. Solution (how do we get there from here)

   Embrace the dynamic languages to make .NET web-development better for everyone
   

------
ACT II
------
*Develop the action*

Numbered items: *Why* they should do the solution
Lettered items: *How* do they adopt the solution

1. Making your entire web-development experience simpler (why)
   
   - Web-apps can be built from scratch using Ruby or Python; both server and client components

   a. Both Ruby and Python can run on the same infrastructure as your ASP.NET solutions (how)
      i.   Existing Ruby web-frameworks on IIS (Rails/Sintra + ironruby.rack)
      ii.  Using ASP.NET directly with and IronPython and IronRuby (MerlinWeb + ASP.NET MVC)
      iii. Help make very simple PHP-like websites (ERB/HAML/SASS/Less on IIS)

   b. Writing browser applications (Silverlight) in Ruby and Python is extremely powerful (how)
      i.   Managed code in HTML-script tags (app-model tour)
      ii.  Interop everywhere: 
           - Use all existing Silverlight APIs (webcam)
           - interop with user-C# code (mandelbrot example)
           - the HTML DOM (photoviewer)
           - and the browser's JavaScript engine

2. Static languages still have to deal with dynamic environments (why)

   - Can seamlessly interoperate with existing .NET code (written in C#/VB/Boo)
   - You don't have to start from scratch to use these languages today; they support using their engines from existing .NET code-bases
   - either drive the application with dynamic languages, and call into static for lower-level things, 
   - or write it in a C# app and call into dynamic language when you need more dynamic behavior.

   a. Test your applications with with Ruby or Python (how)
      i.   Ruby's DSL-abilities Testing with Ruby intro (bacon or roll-your-own)
      ii.  Test server-side web applications - scrape.py, Watir, or SL
      iii. Test your Silverlight application

   b. Using scripting languages in existing C#-based web-applications (how)
      i.   DLR Hosting 101 (include "dynamic")
      ii.  It's all hosting
      iii. Embedding into your own applications for dealing with dynamic data (polyglot)

3. Allowing others to extend your applications will make it more powerful (why)

   a. Public extensibility -- basically building a platform for end-users
      - Facebook
   
   b. Extensibility for yourself
      - 

4. Does this simplify .NET development and open it up to a broader audience?


-------
ACT III
-------

1. Crisis

   Web developers move to other platforms for simpler programming models
   
2. Solution

   Embrace the dynamic languages to make .NET web-development better for everyone

3. Climax

   Simplify .NET web-development with dynamic languages

4. Resolution

   Dynamic languages - in the browser and on the server


============
Random notes
============

FAQ
---

What console are you using?
    
    Console2


Average audience member
-----------------------

- What do we know about this person? How do they make decisions?
  o .NET developer, probably C#. Maybe VB if background is VB6 and before.
  o Designer interested in simple coding
  o Asks self question: "can I use it from Visual Studio?"

- What is the audience's problem?

  o Will ask: What is in this for me? Why do you think this is important for me? Why should I care?
    - increased productivity
    - way to get simple things done
    - simplify your current solutions
    - give others an easy way of customizing your work
  
- Collages
  o Where?
  o When?
  o Who?
  o What?
  o Why?
  o How?


Concepts
--------
- Both languages are compliant implementations
- web-apps can be built from scratch using Ruby or Python; both server
  and client components.
- Can even be used to simplify writing web-apps
- can seamlessly interoperate with existing .NET code (written in C#/VB/Boo).
- you don't have to start from scratch to use these languages today;
  they support using their engines from existing .NET code-bases.


Technologies
------------
- Silverlight: Ruby and Python in the browser
- ASP.NET MVC with IronRuby
- IronRuby.Rack
- ASP.NET with IronPython
- ERb/Haml/Sass/Less in IIS (PHP+)


Why do .NET developers care
---------------------------
- Testing
- Scripting (end-user extension)
- Embedding (polyglot)
- Application development/productivity


It's all embedding
------------------
- These languages are built *on* .NET; today C#/VB are not ... the compile .NET programs.
- Therefore, dynamic languages are different in that they need to run in a .NET application
- So, all Ruby or Python code runs by hosting the languages inside a .NET application
- We do things to make this seamless in specific environments
  o ir.exe/ipy.exe: .NET programs which host the language and run the code in a command-line
  o ipyw.exe: runs scripts in a console-less program for Windows applications.
  o Microsoft.Scripting.Silverlight.dll: entry-point for Silverlight applications which
    runs script tags and scripts inside the XAP
  o IronRuby.Rack: runs rack-based applications on IIS
  o Microsoft.Web.Scripting.dll: runs code against ASP.NET
  o Microsoft.Mvc.Scripting.dll: runs code against ASP.NET MVC
- However, we can't provide "runners" for every environment that will spring up,
  so we allow you to use the same APIs that these runners use in your own apps.
- What this can enable?


Scripting or Dynamic
--------------------
This word just gives me a bad taste anytime I hear it; scripts generally sound
like a brittle thing that is just used to piece your build system together, but
not something that you would use in your actual production code. But, really the
term just means the language is meant to drive some existing system, like
AppleScript.

Dynamic languages just define the way the language is generally used; dynamic
languages are generally compiled from source code on execution, and may also
have a dynamic-type system, but they are real languages. Their development
experience can be superior to static languages, as they usually provide
easy ways to modify live applications, making it easy to experiment with
solutions.

Today, "scripting" languages are actually powerful, expressive, and performant.
I personally separate "scripting" and "dynamic" languages; "scripting" languages
are like batch/bash/tcl, while "dynamic" just 
Granted, C# will compute a fractal faster than Python, but you're not always
doing that type of expensive computation. 


Open questions
--------------
- Motif: pumping iron
  o Arnold Schwarzenegger
    - California governor
    - losing money
    - Accent
      ~ Conan O'Brien making fun of him
    - Terminator
  o IRON: it runs on .net or implementation running on .net
  o Working out
  o getting fit, lean, quick, fast
  o http://www.funnyanimalsite.com/pictures/Lions_Working_Out.jpg
  o Yoga
  o Atomic symbol Fe
  o IronMan
  o Ironing - smoothing out
  o bridges
  o sword, crosses
  o skillet, waffelmaker
    - http://www.treehugger.com/files/2007/05/my_type_of_appl.php
  o Iron Maiden
  o Iron Sea
  

