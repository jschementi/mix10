=================================================
Pumping Iron on the Web - IronRuby and IronPython
=================================================

Welcome to “Pumping Iron” on the web! I’m Jimmy Schementi, program manager of
IronRuby and IronPython at Microsoft, lead dev on “Gestalt” – the Silverlight
DLR integration, as well as everything else web-related with the “Iron” languages.
Today I’ll be talking about how you can embrace dynamic languages on Microsoft’s
web platform; be it on the web-server (IIS) or in the web-browser (Silverlight),
and even in your existing applications.

**Quick detour**: Jim Hugunin and John Lam have both been quoted
as saying “Iron” stands for different acronyms; “Implementation running on .NET”
and “It runs on .NET”, respectively. Officially, it’s Jim’s definition, as he
stated that at a PDC talk, and he chose the name for IronPython. A 
`Port 25 <http://port25.technet.com/archive/2006/06/01/2565.aspx>`_
interview explains more, but I should just put this on the websites and put the
wondering to rest …

Anyway, let’s really get started. Like I said, this entire talk is about the why
and how of .NET developers embracing dynamic languages. And here’s my rational for
why we as a developer community should care:

Web developers seem to gravitate around simpler programming models for the web.
Initially the platform itself was pretty attractive (instant deployment), but 
the simple UI mark-up system (HTML) and a simple scripting language (JavaScript) 
still make it easy for people to build the the amazing improvement websites we’ve
seen over the last 20 years. But developers are still evolving the web development
model; server and client frameworks have become a very popular way of building web
apps – very rarely does a website have no server side or client side dependencies.
These frameworks make the entire experience simple, and focus on getting things
done.

However, developers are really getting things done because they can choose how
they do it -- “I want to use Python so I’ll use TurboGears”, for example. Really,
the power and simplicity that these web frameworks are achieving is because they
stand on the shoulders of these powerful and expressive dynamic/scripting languages;
giving the frameworks the unique ability to model the “web” as they see fit.

Now, we’re all .NET web-developers and designers, and we want to get things done
too … so if getting things done is essentially the result of programming language
choice, what choice do we have? C# and VB, and traditionally more static verbose
languages – that’s not to say that they’re bad, but just not very simple to use.
Take a look at the other languages mainly used on the web … they’re all dynamic
languages! Why static vs. dynamic? Why can’t they exist together? If only .NET
provided some language choice for it’s developers we could have all the languages
be used together, and benefit from the amazing work being done by dynamic language
developers … oh wait, it does!

While the CLR is truly common enough to support multiple language implementations,
the DLR has paved the way for more dynamic languages to run on the CLR, and the
future of dynamic languages on the CLR is through the DLR. So, might as well learn
early!

So embracing dynamic languages is possible on .NET, but why would you want to do
it? I’ll discuss making your entire web-development experience simpler, focusing
on specific pieces of your application where things are just dynamic, and lastly
the benefits opening up your applications to extensibility brings.

--------------------
Making it all simple
--------------------

Ruby and Python can be used both on IIS as well as in Silverlight to build apps
from start to finish; let’s first look at the server. Both languages can run on
the same infrastructure as your ASP.NET apps, making deploying them no different
than any other ASP.NET app.

Because IronRuby is a highly-compatible implementation of the Ruby language, it
is able to run applications written with the “Ruby on Rails” web-framework, and
also supports deploying on IIS.

DEMO: Show Rack and Sinatra examples

Thought this deployment method uses ASP.NET, the web-frameworks don’t use any of
ASP.NET’s features directly. If you’d like to use ASP.NET, IronPython supports
running actual ASP.NET apps today.

DEMO: Show MerlinWeb

These still feel a bit complicated to me, though. What if I wanted to write a
VERY simple website? ASP.NET with IronPython is probably the simplest, but
can we do better? Like maybe PHP-style apps on IIS?

DEMO: Show ERB

So that’s a solution for the server, but how in the browser? This is a very
exciting topic for dynamic languages, as they make writing Silverlight
applications just as easy as they were in Silverlight 1, but with the power
of .NET.

DEMO: app-model tour

Let’s highlight some interesting features:

- Very easy to use existing Silverlight APIs
- interop with user-C# code
- the HTML DOM and the browser's JavaScript engine

----------------------------------------------
Consuming dynamic things from static languages
----------------------------------------------

- Can seamlessly interoperate with existing .NET code (written in C#/VB/Boo)
- You don't have to start from scratch to use these languages today; they
  support using their engines from existing .NET code-bases
- either drive the application with dynamic languages, and call into static
  for lower-level things
- or write it in a C# app and call into dynamic language when you need more
  dynamic behavior

Test your applications with with Ruby or Python
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Ruby's DSL-abilities Testing with Ruby intro
2. Test server-side web applications - scrape.py, Watir, or SL 
3. Test your Silverlight application

Using scripting languages in existing C#-based web-applications
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. DLR Hosting 101
2. It's all hosting 
3. Embedding into your own applications for dealing with dynamic data (polyglot)

Allowing others to extend your applications will make it more powerful
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Public extensibility -- basically building a platform for end-users
   - Facebook 
2. Extensibility for yourself 


**Does this simplify .NET development and open it up to a broader audience?**


- Web developers move to other platforms for simpler programming models
- Embrace the dynamic languages to make .NET web-development better for everyone
- Simplify .NET web-development with dynamic languages
- Dynamic languages - in the browser and on the server

