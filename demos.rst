============
DEMO SCRIPTS 
============

----------------------------------------------
Application development with dynamic languages
----------------------------------------------

Server-side: IIS
++++++++++++++++

1. [ ] Ruby on IIS (running Rails and other Ruby frameworks)
  
  - P0 TODO: Rails + DB
  - P1 TODO: Pictures app running

  a. Sinatra + Ruby/Python 101

     Ruby itself has very simple syntax, and web-frameworks have been built
     to make web-development really simple. For example, Sinatra is a mini-web-framework
     made to minimize the amount of code required to respond to web requests:
 
         get '/' do
           "Hello, World"
         end
 
     This does exactly what it says; when a get request happens for '/', render 
     "Hello, World". This highlights Ruby's DSL abilities too; get looks like a 
     keyword here, though it's really just a method call with '/' as the first
     argument ... yes Ruby lets you omit parenthesis from method calls too
     (any VB fans out there?). The do-end block is syntactic sugar around passing
     a lambda as the last-argument to the "get" method; all Ruby methods take
     an arbitrary "block" of code between do-end or {} (yes, curly braces 
     for all those C# fans ... it could have been written like this):
 
         get('/') {
           "hello world"
         }
       
     Inside that block is what happens on each request, and it's just the string
     "Hello, World". In Ruby, the last statement of any block (methods included)
     is implicitly the return value of the method.
 
     Though these features sound kinda arbitrary by themselves, if I were to
     write this with non-Ruby language features, it would lose it's character:
 
         def index()
           return "Hello, World"
         end
         get('/', method('index'))
 
     This defines a Ruby method "index", which explicitly returns the string
     "Hello, World", and then calls the get method directly with the first
     argument being the URL and the second argument being an explicit pointer
     to the "index" method. While this might be closer to how the programming
     language tackles problems, it's not how the programmer thinks.
 
     Now, not to leave Python out of this love-fest, Python can make this
     look very pretty as well, but in her own special way:
     
         @get("/")
         def index(resp):
             resp.write("Hello, World")
 
     Here the index method is created, which explicitly accepts the request
     as an argument; Python's all about not introducing any magical variables,
     unlike Ruby, so the entire request would probably be given to Python.
     The index method would probably write to the req using a write method.
     Then the method would be "decorated" with the get method, which would tell
     the web-framework that index represents a get-request for "/".
 
     A decorator in Python is basically a function that accepts a
     function and returns a function, so get in this case would be
     defined something like this:
     
         def get(uri):
           def __get(resp):
             sinatra.register('get', uri, resp)
           return __get
 
     That's code that would be part of my fictional Python Sinatra fx,
     not something you as the consumer would write.
       
     Another way of looking at it is without decorator:
 
         def index(resp):
             resp.write("Hello, World")
         get('/', index)
 
     The thing to note is that it's a bit more readable than Ruby,
     and almost equivalent to the decorator way, except for the 
     order of "get" in the code. You'll also see that getting a method
     pointer is much cleaner than Ruby ('index' vs 'method(:index)');
     in Ruby 'index' would call the method, since Ruby allows method
     calls with or without parens, where Python uses parens to indicate
     a method call.
 
     <start http://ironruby.info>
 
     Quickly back to Sinatra: the IronRuby team actually uses Sinatra
     to power http://ironruby.info, our compatibility reporting website.
     A machine runs the compatibility suite against the latest source
     code every night, and generates data into a database which this
     site pull out and displays.

  b. Ruby on Rails - Databases with Ruby

     One of the most popular (or most buzzed) web-frameworks is Rails,
     which is just a collection of libraries for structuring your
     web-application, and Ruby gives it the power to make it so nice.
     Rails uses the Model-View-Controller pattern for organization,
     so any ASP.NET MVC people will find this familiar:

         class PostsController < ActionController::Base
           def index
             @posts = Post.all
           end

           def show
             @post = Post.find(params[:id])
           end

           def create
             @post = Post.new(params[:post])
             unless @post.save
               flash[:error] = @post.error
               redirect_to :index
             end  
           end

           # ...
         end

     Each method inside a class (inheriting from ActionController::Base)
     maps to a certain URL and HTTP verb: "index" maps to a "GET /posts",
     show maps to a "GET /posts/<id>", "create" maps to a "POST /posts",
     "destroy" maps to a "DELETE /posts/<id>", etc. Unlike Sinatra, Rails
     uses "convention" to map a request to it's actions.

     While this is very nice, Rails really shines when it comes to
     interacting with the database through it's ActiveRecord library.
     ActiveRecord maps Ruby classes to database tables, and provides
     an Ruby API for interacting with the database

         class Post < ActiveRecord::Base
           has_many :comments
         end

         class Comment < ActiveRecord::Base
           belongs_to :post
         end
         
         class CreateDB < ActiveRecord::Migration
           def up
             create_table :posts do |t|
               t.string 'title'
               t.text 'body'
             end
             create_table :comments do |t|
               t.text 'body'
               t.integer 'post_id'
             end
           end

           def down
             drop_table :posts
           end
         end

     This is all the code that is required to map your Ruby classes to
     the database, as well as create the structure of the database. It
     dynamically provides getters/setters for the table, as well as
     sets up foreign-keys and relationships based on conventions
     (belongs_to :posts assumes that the table has a 'post_id' field).

     And you can get a taste of how you interact with the database by looking
     at the controller's method bodies; can you guess what "Post.all" does? :)
     Translates to the "SELECT * from posts" SQL query, since the "Post"
     object is mapped to a database table. Also, Post.find(<id>) does a
     "select * from posts where id=<id>", etc.

     Ruby's ability to make things simple has made a name for it.

     Also, because IronRuby is a very-compatible Ruby implementation, and
     because ASP.NET is very customizable, we are able to run Ruby-based
     web-frameworks, like Sinatra and Rails, on IIS through IronRuby. This
     is the best Windows-based Ruby deployment strategy, as it takes
     advantage of IIS's integrated pipeline that ASP.NET plugs into.

     <show Pictures>
     
     For example here is a pretty substantial Rails application running
     on IIS.

2. [ ] ASP.NET MVC with IronRuby

  - P1 TODO: basic sample app
  - P1 TODO: port an existing sample app?
    
  [x] ASP.NET with IronPython
   
  - http://ironpython.net/download/aspnet-201001

  a. ASP.NET MVC and IronRuby

    Now those were all Ruby-based web-frameworks, but what about ASP.NET?
    Can dynamic-languages make ASP.NET simpler too? Sure!

    <show ironmvc source>

    IronRuby has integration with ASP.NET MVC, so you can write your
    controllers and views in Ruby.

    <show controller>

    <show view>

    This integration was built by a bunch of people, including myself,
    Phil Haack, and Ivan Porto Carrero -- a IronRuby MVP who has maintained
    and evoloved it single-handedly for the last year. Oh, the power of
    open-source :)

  b. ASP.NET and IronPython

    IronPython directly integrates with ASP.NET as well, letting you write
    your ASPX code-behind files in Python.

    hello-webforms.aspx:

        <%@ Page Language="IronPython" CodeFile="hello-webforms.aspx.py" %>
        Enter your name:
        <asp:TextBox ID="TextBox1" runat="server">
        </asp:TextBox>
        <asp:Button ID="Button1" runat="server" Text="Submit" OnClick="Button1_Click"/>
        <p>
            <asp:Label ID="Label1" runat="server" Text="Label">
            </asp:Label>
        </p>

    hello-webforms.aspx.py:

        def Page_Load(sender, e):
            if not IsPostBack:   
                Label1.Text = "...Your name here..."

        def Button1_Click(sender, e):   
            Label1.Text = Textbox1.Text


    Because of ASP.NET's events-based API (rather than a response-based API like
    Sinatra/Rails), Python methods can automatically hook events by using the
    <object>_<event-name> convention, and all server-side controls with "ID"s
    ends up being a variable avaliable to the Python module. And application-level
    event hooks can go in Global.py. But it's really nice to write
    Language="IronPython" at the top. =)

    <show picture album>

    Here's a simple gallery app; looking at the file-system and giving you a
    gallery of thumbnails/images, resizing the images on the fly, all written
    in Python.

    In the ASPX page, Python can be used in-line as well, kinda like PHP.

    <TODO PHP-like code>

    It can also interact with the controls:

        <asp:Repeater ID="ThumbnailList" runat="server">
          <ItemTemplate>
            <a href='<%# Link %>'>
              <img alt='<%# Alt %>' src='<%# Src %>' width='<%# Width %>' height='<%# Height %>' style='border:0' />
            </a>
          </ItemTemplate>
        </asp:Repeater>

    The <%# %> syntax lets run Python code in the context of the
    ASP.NET control's data source. The repeater's data-source was set
    to a list of IMAGETAGS (a python class), which has all those fields
    on it.


3. [x] ERB/HAML/SASS on IIS

  - http://localhost/HampExample: C:\dev\orphanage\aspnet-haml
  - P1 TODO: more complex HTML/CSS example

  While running Ruby or Python code behind the scenes is great, sometimes
  a site just requires HTML + some server-side processing, and server-side
  includes are not powerful enough. I'm talking really about what PHP was
  built for; generating HTML with simple server-side programming language.
  Can Ruby do that?

  The common scenario of a header + body + footer is actually really nice
  in Ruby:

  template.erb

      <h1>My Site / <%= page %></h1>
      <%= yield %>
      <p>
        &copy; Jimmy Schementi
      </p>

  index.erb
    
      <h2>Welcome</h2>
      <% 10.times do %>
        Welcome
      <% end %>!

  about.erb

      <h2>About Me</h2>
      <p>Blah blah blah ...</p>
    
  gen.rb

      template = ARGV[0] || 'template.erb'
      files = ARGV[1..-1]
      require 'erb'
      files.each do |file|
        @output = ''
        ERB.new('template.erb').result({:page => file}) do
          ERB.new(file).result(binding)
        end
      end


Client-side: Silverlight
++++++++++++++++++++++++

4. [ ] Python and Ruby in browser app-tour

  - basic hello-world stuff, inline XAML, console, loading from zip file
  - PyCon start.html

  a. Basic Hello World

     These Ruby and Python implementations also work in the browser, thanks to
     Silverlight. In-fact, they are hands-down the simplest way to develop a
     Silverlight application. This is not only because of how expressive the
     programming languages are; the integration with Silverlight doesn't fight
     how the web works. For example, here's an entire Silverlight app
     which just writes a message into the HTML page, written in Python:

         <html>
           <head>
           <script type="text/javascript"
                   src="http://gestalt.ironpython.net/dlr-latest.js"></script>
           </head>
           <body>
             <div id="message"></div>
             <script type="text/python">
               document.message.innerHTML = "Hello from Python!"
             </script>
           </body>
         </html>

     DLR-based Silverlight applications let you write HTML script-tags
     in other languages than JavaScript, but in a way that works cross-
     browser and cross-platform; the languages work in Moonlight as well.

     Both inline and script-src tags are supported:

         <script type="text/ruby" src="foo.rb"></script>

  b. Console

         Say hi to <input id="name" type="textbox" /><input id="go" type="button" value="Go!" />
         <div id="message"></div>
         <script type="text/ruby">
           document.go.onclick do |s,e|
             document.message.innerHTML = "Hello, #{document.name.value}"
           end
         </script>

  c. XAML

     The example above created a 1x1 pixel Silverlight control on the page;
     by default the HTML-page is the default UI. However, you can use Silverlight's
     graphics as well by using script tags:

         <script type="application/xml+xaml" id="xaml1" width="100" height="100">
           <Canvas>
             <TextBlock Name="message" Text="Loading ..." />
           </Canvas>
         </script>
         <script type="application/ruby" class="xaml1">
           xaml1.message.Text = "Hello from Ruby!"
         </script>

     Here a HTML script tag was used to embed XAML directly in the HTML page,
     and then a Ruby script modified the objects loaded from XAML.

     Let's take that say-hello example from before, and make the visualization
     a bit prettier. So, instead of writing the message to the HTML page, let's
     load a nice graphic and talk-bubble animation, created in Adobe Illustrator,
     and exported into XAML.

         Say hi to <input id="name" type="textbox" /><input id="go" type="button" value="Go!" />
         <script type="application/xml+xaml" src="mushroom.xaml" id="xaml1" width="100" height="100"></script>
         <script type="application/ruby" class="xaml1">
           document.go.onclick do |s,e|
             xaml1.message.Text = document.name.value
           end
         </script>

     Also, there's an blinking animation defined in the XAML, but if has to be
     initiated from code; let's do that from Python, because we can =)

         <script type="application/python" class="xaml1">
             xaml1.blink_animation.Start()
         </script>

  d. Gotchas

     1. OOB: because this depends on the HTML page, running apps out of browser
        in this way is not supported. However, DLR apps also support an in-XAP
        programming model, and that will work fine with OOB.
     2. Embedded Resources: because there are no DLLs in this application that
        the user has control over, anything which depends on the user embedding
        DLL resources will require a DLL souley for "housing" the resource, like
        custom fonts (breaking change from SL2 to SL3). 
     3. XAML x:Class: this attribute must point to a "static" classname, so
        if you load XAML onto a UserControl, the value must be "System.Windows.Controls.UserControl",
        not your derived Python class-name.
     4. JavaScript differences: TODO


5. [x] User-C# code (mandelbrot example)

  - http://localhost/pycon2010/mandelbrot.html

  a. Using user-C# code

     Though this was hinted at throughout the talk, it's not been specifically
     addressed; both the Iron-language's sweet spots are it's first-class 
     integration with the CLR, and in-tern they get direct access to all source
     code written for the CLR; including the entire .NET framework and all CLR-
     based user-code, like your own C#, VB, Boo, F#, etc. And this is no exception
     in Silverlight.

     <show mandelbrot>

     A use case for doing this is if you choose to write your entire application
     in Python, for productivity, simplcity, and maintainability reasons, but 
     a part of the application has a very high-performance requirement, like
     something that crunches numbers; that piece can be writtin in a static
     language, which can do computaitons very fast. This doesn't mean that
     dynamic languages are too slow for normal application development, but
     the overhead of dynamic method lookup and other dynamic-language features
     are amplified when doing millions of iterations.
  
     Note: For fractal computation, it turns out that IronPython it one of the
     fastest scripting languages:
     http://mastrodonato.info/index.php/2009/08/comparison-script-languages-for-the-fractal-geometry/

     For example, this application is written in IronPython, except for the
     fractal bitmap generation, that is computed using C#. Calling into the C#
     code from IronPython is very simple; just add a reference to the DLL,
     import the namespace just like it's a Python module, and use classes/methods
     using Python's syntax:

         import clr
         clr.AddReferenceToFile("bin/mandelbrotbase.dll")
         import mandelbrotbase

         mandelbrotbase.GenerateMandelbrot(
           int(self.Content.FractalArea.Width),
           int(self.Content.FractalArea.Height),
           self.CurrentXS, self.CurrentYS,
           self.CurrentXE, self.CurrentYE
         )

     This direct integration makes it trivial to just begin writing your
     application in a dynamic language, and then decide to convert any
     performance-sensitive sections to a static language.

6. [x] Built-in Silverlight APIs (webcam)

  - http://localhost/pycon2010/webcam.html

  a. Using built-in Silverlight APIs

     The previous example used Silverlight's WritableBitmap to render the
     mandelbrot bitmap, also showing that IronPython can work directly with
     Silverlight APIs, and not just user-code. Another useful feature of
     WritableBitmap is being able to attach any bitmap-producing stream,
     like a Webcam, and doing that from a dynamic language is trivial:

         vidBrush = VideoBrush()
         vidBrush.SetSource(_CaptureSource)
         xaml.WebcamCapture.Fill = vidBrush 
         
         if CaptureDeviceConfiguration.AllowedDeviceAccess or CaptureDeviceConfiguration.RequestDeviceAccess():
            _CaptureSource.Start()

     Working with Silverlight's APIs is just as easy as using the language's
     syntax for methods, classes, etc; again these languages integrate directly
     into the .NET framework, giving you the best of both words: tremendously
     powerful .NET libraries and expressive scripting languages.

     Here's the webcam demo that Tim Heurer put together:

     <show webcam>

7. [x] HTML DOM (photoviewer) and browser's JavaScript engine (lightbox, html5)

  - http://localhost/ruby/photoviewer/
  - P0 TODO: make invoking JavaScript functions work from Python

  a. Browser DOM
 
     In the earlier examples, the HTML DOM was used for simple UI, but it can be
     used for the entire application, just like JavaScript+HTML apps do today.
     However, Ruby's object-oriented features and it's templating library (ERB)
     that was shown earlier make it a great client-side HTML scripting language.
  
     <demo it>

     First off, the application is nicely divided into an Photoviewer::App class
     which handles the application's logic, while Photoviewer::View handles
     all the presentation logic. So, scripting languages have the object-oriented
     features you're used to from other .NET languages.

     Also, because Ruby has an existing standard library (written in Ruby), that
     resource also becomes available in Silverlight. That ERB library we used
     to template HTML on the server can also be used to template HTML on the
     client:

         <% if @flickr.stat == "ok" && @flickr.photos.total.to_i > 0 %>
           <div class='images'>
             <% @flickr.photos.photo.each do |p| %>
               <div class='image'>
                 <a href="<%= flickr_source(p) %>.jpg"
                    title="<%= encode("<a href='#{flickr_page(p)}' target='_blank'>#{ p.title }</a>") %>"
                    rel="lightbox[<%= @tags %>]"
                 ><img src="<%= flickr_source(p) %>_s.jpg" /></a>
               </div>
             <% end %>
           </div>
         <% else %>
           No images found!
         <% end %>

  b. JavaScript interop

     Using one of these languages in the browser doesn't mean you have to abandon
     all your JavaScript code and start over; they can be used together. For example,
     the photoviewer uses a JavaScript library called "lightbox" to display the large
     version of each image when clicked on. And that library can be set up
     directly from Ruby:

         if document.overlay && document.lightbox
           document.overlay.parent.remove_child document.overlay
           document.lightbox.parent.remove_child document.lightbox
         end
       
         window.eval "initLightbox()"


-----------------------------------------------------
Using dynamic languages in your existing applications
-----------------------------------------------------

Up until now I've discussed how to use dynamic languages to power both the
server-side as well as the client-side of your web-application, but what if
you want to apply these methods to solve certain problems in an existing
application?

Testing
+++++++

A low-risk, high-benefit use of dynamic languages in your existing
applications is for testing. This helps make the act of writing tests
simpler, and quite possibly more fun, encouraging your team to actually
maintain the test suite. =)

8. [x] Testing with Ruby

  - "testing" directory

  a. Roll-your-own test fx, and more Ruby DSL explorations

     Before looking at how to test web-app, let's take a brief look at what a 
     test written with RSpec, and popular Ruby testing framework, looks like:

         describe '.NET Stack instantiation' do
           it 'can create an empty stack' do
             stack = Stack.new
             stack.should.be_kind_of Stack
             stack.count.should == 0
           end

           it 'can create a stack from an array' do
             stack = Stack.new [1,2,3]
             stack.should.be_kind_of Stack
             stack.count.should == 3
           end
         end

     Note: there are Ruby testing frameworks that look a bit more like what you
     might be used to. The following is an equivalent test written with test/unit,
     and this will give you a better idea of the structure of the above example:

         class DotNetStackInstantiation < Test::Unit::TestCase
           def test_creating_empty_stack
             stack = Stack.new
             assert(stack.kind_of? Stack)
             assert(stack.count == 0)
           end

           def test_creating_stack_from_array
             stack = Stack.new [1,2,3]
             assert(stack.kind_of? Stack)
             assert(stack.count == 3)
           end
         end

     The RSpec snippet almost reads like english, making it very clear what the
     intended behavior of Stack is. Also, it shows the power of Ruby for creating
     internal DSLs; a "language" built out of the constructs of an existing language.
     describe" and "it" look like keywords, but in-fact they are really just methods,
     because Ruby has optional parameters (as we discovered earlier with Sinatra).
     Using actual strings as the test name, rather than a method name, allows
     you to describe the test accurately. Each object has a "should" method which
     makes any subsequent calls part of an assertion, making it very obvious
     which value is the "expected" value and which is the "actual".

     The crazy thing is how little code is required to make that work; 26 lines of
     Ruby. The key points are that "yield" executes the do-end block passed to 
     a method, and the "should" method is added to every object, turning 
     any subsequent methods calls into an assertion.

         def describe string
           puts string
           yield 
         end

         def it string
           puts "  #{string} "
           yield
         end

         class Object
           def should
             PositiveAssertion.new(self)
           end
         end

         class PositiveAssertion
           def initialize lhs
             @lhs = lhs
           end
           def == rhs
             print @lhs == rhs ? '.' : 'F'
           end
           def be_kind_of type
             self.class.new(@lhs.class) == type
           end
         end

     However, please don't use this example as your real testing framework, and
     then get mad at me when it doesn't have a feature you want. =)
     RSpec, Bacon, or test/spec are much more mature testing frameworks that
     support this syntax.

     Anyway, for just a "whoa-cool" demo, let's run the identical tests on the
     desktop as well as in Silverlight. =)

9. [ ] Test server-side web applications

  - P0 TODO: Watir on Ruby
  - P1 TOOD: scrape.py

  You could use this same technique to test your server-side web applications,
  but they can also be used to actually do end-to-end testing; actually sending
  a web-request to your server, and testing what it sends back. Even better,
  there are libraries for controlling individual browsers with Ruby, so you can
  make sure your applications work across them.

  TODO!!!


10. [ ] Test your Silverlight application

  - P0 TODO: get sample working

  These techniques can also be used to test Silverlight applications, even if
  they are written in a static language.

  TODO!!


Hosting
+++++++

IronRuby and IronPython are built on-top of the Dynamic Language Runtime, which
is comprised of many parts, one of which being a **.NET hosting API**, allowing
you to embed a scripting language in any old .NET app.

Now we come to the "ah-ha!" moment of the talk; **everything** you've seen today
is made possible by this API. Keep in mind these languages are built *on* .NET,
so their implementations are accessible from any .NET language. C# and VB today
are not built on .NET; they just compile programs to run on .NET, which is why
you can't easily host C# today.

Here's the catch; since these language engines are built on .NET, they need to
run *in* a .NET application. So, **all** Ruby or Python code runs by hosting the
languages inside a .NET application.  We do things to make this seamless in
specific environments: for example, ``ir.exe`` and ``ipy.exe`` are both .NET
programs which host the language and run the code in a command-line, minimcing
ruby.exe and python.exe's behavior. Here are the other hosts provided:

- ``ipyw.exe``: runs scripts in a console-less program for Windows applications
- ``Microsoft.Scripting.Silverlight.dll``: entry-point for Silverlight
  applications which run HTML script tags and scripts inside the XAP
- ``IronRuby.Rack.dll``: run rack-based applications on IIS
- ``Microsoft.Web.Scripting.dll``: run Python in ASP.NET
- ``System.Web.Mvc.IronRuby``: run Ruby in ASP.NET MVC

However, we can't provide "runners" for every environment that will spring up,
so we allow you to use the same APIs that these runners use in your own apps.
These APIs have been kept very simple, as we want any .NET developer to be able
to use a DLR scripting language in their applications.

But why embed a scripting language into your application? The main scenario
is to scripts as an extensibility mechanism, either internally or as
functionality you provide for your end-users. Here are a few concrete examples
of what scripts could be used for:

- An advanced search / filter
- High-level business logic
  o computing prices of items, applying discounts, etc
    - any type of rules engine; system changes behavior based on external data
- Customizing a single codebase for different clients
- Add-ons for end-users to make your application better

Let's show you how to do the basics, and hopefully that will spark your
imagine to think up other cool use-cases.

11. [x] DLR Hosting 101

  a. C# ASP.NET app with embedded Ruby and Python

     Create a new web application project in Visual Studio, and open the 
     Default.aspx.cs page.

     <>

     The normal "Hello, World" would be to place a label on the page and
     set it's text from code ... let's do that with Python instead.

     First add, references to the necessary DLLs to host IronPython:

     <add IronPython.dll and Microsoft.Scripting.dll>

     Then you can write the 5 lines of code to get this all working:

         var runtime = ScriptRuntime.CreateFromConfiguration();
         var engine = runtime.GetEngine("IronPython");
         dynamic scope = engine.CreateScope();
         scope.page = this;
         engine.Execute("page.Message.Text = 'Hello from Python!'", scope);

     There are basically three types you need to know; a ScriptRuntime, a ScriptEngine,
     and a ScriptScope.

     - ScriptRuntime is a level of encapsulation for your scripts; it represents
       the DLR scripting runtime, and all script operations go through it.

     - ScriptEngine is the type that is returned from ScriptRuntime.GetEngine;
       it represents a DLR-language. In this case, we asked for the language by
       name, as that's the easiest way to keep it easily configurable, but the
       downside is you need language config info in app.config. If you only want
       to depend on a closed set of languages, you can use
       IronPython.Hosting.Python.CreateEngine(), which does all the setup for
       Python for you.

       The ScriptEngine enabled you to execute code in that language, in a 
       variety of ways, from the basic engine.Execute method (eval), or
       being more fine-grained engine.CreateScriptSourceFromString(code).Compile().Execute(),
       which parses the file, compiles it, and then executes it. Code can be
       executed against a ScriptScope to set initial state and share state
       between executions ...

     - ScriptScope defines the state for your script; like what variables/methods
       are present. It is a dynamic object, so you can do things like
       "scope.page = this", and that will set the "page" variable for scripts
       that execute against the scope. In downlevel .NET frameworks, you'd have
       to use scope.SetVariable("page", this).

  Slight aside: since these APIs are .NET based, the dynamic languages themselves,
  can consume them to run other dynamic languages! =) For example, here's Ruby
  executing Python code:

      require 'IronPython'
      require 'Microsoft.Scripting'
      include Microsoft::Scripting::Hosting
      include IronPython::Hosting

      python = Python.create_engine
      scope = python.create_scope
      python.execute "
      class Foo(object):
        def bar(self):
          print 'Look ma, white-space-sensitivity!'

      ", scope
      python.execute "Foo().bar()", scope

  What's also interesting is the dynamic languages can communicate between
  eachother just as easily; here's Ruby calling Python code:

  foo.py
      
      class Foo(object):
          def bar(self):
              print 'Look ma, white-space-sensitivity!'

  bar.rb

      foo_module = IronRuby.require 'foo'
      foo_module.foo.bar


12. [x] ShapeScript

  - desktop version: \dev\rubyconf2009\sketchscript
  - P1 TODO: get working in Silverlight
  - P1 TODO: Python examples as well

  a. Extending an actual application with scripts

     TODO
