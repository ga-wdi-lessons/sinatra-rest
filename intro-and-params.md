# Intro To Sinatra

## Learning Objectives

- Describe what Sinatra is and what it is used for
- Build a Sinatra app that responds to HTTP requests
- List the HTTP request verbs
- Distinguish between a route and a path
- Define routes with URL parameters and access those parameters
- Access data from the params hash in sinatra


## Intro (10 minutes)

In unit 1, we learned all about HTML/CSS/JS, and the web. It was awesome, we got
to spend our time building sweet apps and games like War, Concentration, etc.

In this most recent unit, we learned about Ruby, an awesome language that can
run outside the browser, and makes it easy to save data to a DB and write our
programs using OOP.

But you probably noticed that we've been stuck writing boring CLI apps that
would torture most of our users:

-----

![terminal CLI app](assets/terminal-app.png)

-----

How are we going to put the chocolate of our awesome Ruby skills together with
the peanut butter of our ninja-like HTML skills?

How can we build applications that are backed by *data in a database*, but have
an interface on the *web*?

### Enter Sinatra

![Sinatra](http://www.driehausmuseum.org/img/events/Frank-Sinatra.jpg)

Well, maybe not that Sinatra...

## What is Sinatra?

Sinatra  is a **framework** for quickly creating web applications in Ruby with
minimal effort.

Other ways include Rails, Express / Node.js, Django, PHP, etc.

But what do we mean by *web application*?

### What are Web Apps?

A web app is a *program* that can receive HTTP requests (i.e. users can access
it from their browsers). Requests could be something like:

* show me a list of all artists in this app
* show me more info about the artist named 'Drake'
* here's my information, please register me as a user
* show me all tenants in the apartment at '123 Walnut Dr.'
* place the tenant 'Andy Kim' into the apartment at '42 Wallaby Way, Sydney'
* turn on the lights in my living room

It then *does something(s)* to *process* that request. That could be:

* retrieving the data necessary from a database
* adding info / modifying info in a database
* sending an email
* sending a command to a device connected to the server to turn on some lights

Finally, any good web application is going to *respond* to the request. That
response is almost always going to be an HTML page. Usually, that HTML page
*isn't* an existing HTML file. Instead, the server uses information it has to
*build* the page according to a *template* (more on that later).


## Our first Sinatra app!

Let's create a folder to work in. Create a file called `app.rb` and `Gemfile`.
The file name doesn't matter, but it's easier if we all use the same one.

```bash
$ mkdir sinatra_intro
$ cd sinatra_intro
$ touch app.rb
$ touch Gemfile
```

Let's make sure we include our sinatra dependency in the `Gemfile`

```ruby
source 'https://rubygems.org'

gem 'sinatra'
```

Then run a `bundle install` in the terminal.

Inside `app.rb`, write:

```ruby
require 'sinatra'

get '/' do
  return 'Hello world!'
end
```

We'll talk about what `get` means later on.

Now, in your console, run the file the way you'd run any Ruby file:

```sh
$ ruby app.rb
```

You should see something like:

```
[2015-10-30 13:57:04] INFO  WEBrick 1.3.1
[2015-10-30 13:57:04] INFO  ruby 2.2.1 (2015-02-26) [x86_64-darwin14]
== Sinatra (v1.4.6) has taken the stage on 4567 for development with backup from WEBrick
```

> See the "has taken the stage" thing? There'll be lots of little Sinatra puns
here and there.

Believe it or not, that's it! You now have a server running on your computer. It
can respond to requests, just like any other server. To test that out, go to
`localhost:4567` in your browser. You should see "Hello world!"

> Note that this isn't a server anyone else can see, but it's still a server.

### Where does that `4567` come from?

This is the **port number** of the server we're using. Don't worry about it too
much -- just know that to access a Sinatra app on your computer you'll always
use `localhost:4567`, unless someone changes it.

## Routes

The `/` in `get '/'` is the *route* to which someone needs to go to make this
bit of Ruby code run. We can have more than one route in our app!

Try going to `localhost:4567/oh_hello` in your browser. What do we see?

You should get a page saying "Sinatra doesn't know this ditty." That's Sinatra's
404 page. It's saying, "I don't know what to do when someone goes to
`/oh_hello`".

### Mini-Exercise: Add another route

Add a second `route`, such that when the user visits `/oh_hello`, they see a
page that says: "You just got pranked! That's entirely too much tuna!"

![Too much tuna!](https://45.media.tumblr.com/1cf4c52877278785e86499c4895b9816/tumblr_n1zzoeYvH41qzhbquo7_500.gif)

Bonus: Make the page include an image tag with a tuna sandwich.

Don't forget to restart your server to test your work!

## But I'm getting tired of quitting and restarting!

Fortunately, there's a gem that will automatically restart Sinatra every time a
change is made to your `app.rb`.

Let's create a Gemfile:

```ruby
source 'https://rubygems.org/'

gem 'sinatra'
gem 'sinatra-contrib'
```

> Sinatra-contrib is a gem that packages a lot of functionality. One of those
functionalities is `sinatra/reloader`, which detects every time you save your
`app.rb` file and restarts the server so that it uses the newest version of the
file.

`bundle install`, and require sinatra's reloader in your `app.rb`:

This should create a `Gemfile.lock`, which you don't need to touch.


## Exercise: Sinatra Games (15 miuntes)

Check out the [Sinatra Games](https://github.com/ga-wdi-exercises/sinatra_games) repo. Try to do as many of these games as you can!

Note: You can use your current sinatra app you've been working in... no need to
create a new one.

Hint: Look up the `Array#sample` method for ones that invove randomness!

Some of them are going to be hard / you don't know enough yet... that's ok... see what you can figure out!

Hint: search [the Sinatra documentation](https://github.com/sinatra/sinatra#routes) near the top for `named parameters` for some clues on how to do the the magic 8 ball one.

## Getting User Input - The Params Hash

### Demo - `gets.chomp`

So it's clear that `gets.chomp` is no way for us to get input from our users.

Instead, we have basically two options on the web:

1. Info from the URL.
2. Info from a submitted form.

Today we're just going to look at option 1. We'll cover option 2 in the forms
lesson.

What does it mean to get info from the URL? Here are some examples (I'm omitting
the `localhost:4567` part):

* `/artists/4/songs`
* `/say_hello/adam`
* `/forecast/20003`
* `/forecast?zip=20003`
* `/forecast?city=washington,dc&date=tomorrow`
* `/artists?name=drake`
* `/search?name=kroll%20show&format=tv`

> What's up with that `%20`? We can't have spaces in URLs, so we have to
*encode* them.

How can we do this in sinatra?


### Named Parameters in the Route

Let's add the following example to our app:

```rb
get '/hi/:name' do
  return "Hi there, #{params[:name]}!"
end
```

Try going to `/hi/you_handsome_devil`. What happens? What if you change the URL?

What does this tell us? **Params is a hash!**. The *keys* are the placeholders
we define in our routes, and the values are the values users include in their
URLs.

### Mini-exercise: Another route w/ Params!

Try creating a new route that doubles a number. When I go to:

* `/double/2`
  * I should see: `4`
* `/double/8`
  * I should see: `16`

### Mini-exercise: Magic Eight Ball

Try to do the Magic Eight Ball challenge from the
[sinatra games](https://github.com/ga-wdi-exercises/sinatra_games) exercise.


### Multiple Params

You can have multiple params in one route:


```ruby
get '/fancy_hi/:firstname/:lastname' do
  "Hi! Your name is #{params[:lastname]}. #{params[:firstname]} #{params[:lastname]}"
end
```

### Routes vs Paths

Some terminology:

There are two parts to every HTTP request that sinatra (and basically any web
framework) cares about: the *VERB* and the *PATH*.

You can't really 'see' the verb. It's usually GET, but not always. It could be:

* GET - for 'getting' info from the server (no data is changed)
* POST - for 'creating' new data on the server (usually by submitting a form)
* PUT - for 'updating' existing data on the server (usually by submitting a form)
* DELETE - for 'deleting' data on the server

The *path* is what you see in the address bar (i.e. everthing in URL after the
  first `/`).

The combination of a VERB + PATH make a *ROUTE*. These little blocks we've been
writing in Sinatra are *routes*. I like to think of them as `features` or
`things my web app can do`.
