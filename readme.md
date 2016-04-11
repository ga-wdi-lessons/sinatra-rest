# Sinatra & REST

## Screen Casts

[Sinatra 1](https://www.youtube.com/watch?v=ddLEgAjsyP0)
[Sinatra 2](https://www.youtube.com/watch?v=4gilpAcCHpI)
[Sinatra 3](https://www.youtube.com/watch?v=8HYqwTGbrIY)
[Sinatra 4](https://www.youtube.com/watch?v=p8ZhfoMsqaM)
[Sinatra 5](https://www.youtube.com/watch?v=K1DtJCOsrpU)

## Parts

Parts

* [intro to sinatra and params](intro-and-params.md)
* [views](views.md)
* [intro to sinatra and params](intro-and-params.md)

## Learning Objectives

- Explain what REST is and why we use it
- Use erb to create reusable views and templates
- Use a global template - layout.erb


## Getting user input

Let's make this app dynamic and have it say your name. So far, the way we'd do
that is by adding `gets.chomp`. Let's try that here:

```rb
require 'sinatra'

get '/' do
  name = gets.chomp
  return 'Hi there, #{name}!'
end
```

Quit and restart Sinatra, and refresh `localhost:4567`.

Notice how the webpage is still loading? The spinner is still going round and
round. It's doing that because the server is all hung up: Ruby won't let it do
anything until we've entered some input for `gets`.

So try writing your name in the Terminal console, then hitting return.

#### We have a way of getting user input... But we'd never use `gets` on an actual web app. Why not?

Because (hopefully) no-one else will be able to access your server log!

**You will never have any reason to use `gets` again.** So don't!


## Path Parameters

Change `get '/'` to `get '/:name'`:

```rb
require 'sinatra'
require 'sinatra/reloader'

get '/:name' do
  return "Hi there, #{params[:name]}!"
end
```

### Experiment 1

Now go to `localhost:4567/yourname`. See your name show up?

#### Now go to what you may have expected, `localhost:4567/:name`. What shows up?

### Experiment 2

Now change it to `get '/test:name'`.

```rb
require 'sinatra'
require 'sinatra/reloader'

get '/say_hi:name' do
  return "Hi there, #{params[:name]}!"
end
```

Go to `localhost:4567/say_hirobin`. You should see "robin" show up!

### Experiment 3

One last experiment: change it to `get '/say_hi:name/hello`.

```rb
require 'sinatra'
require 'sinatra/reloader'

get '/say_hi:name/hello' do
  return "Hi there, #{params[:name]}!"
end
```

Go to `localhost:4567/say_hirobin/hello`. It still shows "robin"!

### Experiment 4

Now change it to `get '/say_hi/:name'`.

```rb
require 'sinatra'
require 'sinatra/reloader'

get '/say_hi:name/hello' do
  return "Hi there, #{params[:name]}!"
end
```

Go to `localhost:4567/say_hi/robin`. Still works?

### The params hash

Putting a colon `:` in front of `name` turned it into a variable called a path
parameter.

This works similarly to Ruby methods and Javascript functions. For example:

```ruby
def say_hi name
  puts "Hi there, #{name}!"
end
```

```js
function say_hi(name){
  console.log("Hi there, " + name + "!");
}
```

This is a `say_hi` method that has an argument called `name`. An argument lets
us pass some data into the method so the method can manipulate the data somehow.

`params` is a hash that is generated with every request made to your server. It
contains any path parameters -- and some other stuff, as we'll see later.

Now try going to `localhost:4567/say_hi` without your name at the end. We get a
404 page. This is because we're telling Sinatra, "Hey, this path will always
have this parameter in it." Sinatra doesn't have a path defined *without* a path
parameter,so it throws an error.

The same thing happens if you go to `localhost:4567/yourname/lastname`: Sinatra
doesn't have a path defined that has two path parameters.

### Moar Paths

Let's make two more paths: one with no path params, and one with two path params.

```rb
require 'sinatra'
require 'sinatra/reloader'

get '/' do
  "Hi there!"
end

get '/:name' do
  "Good to see you, #{params[:name]}."
end

get '/:firstname/:lastname' do
  "Your name is #{params[:lastname]}. #{params[:firstname]} #{params[:lastname]}"
end
```

Try putting some HTML in one of those strings, like `"<h1>Hi there!</h1>"`. It
works!

We already have an app that generates different HTML based on user input. This
is your first back-end app!

Obviously writing `DOCTYPE` and everything else in here would get really
annoying, so later we'll learn how to use Sinatra templates. I imagine writing
all of your HTML in one string ... ugh

```ruby
require 'sinatra'
require 'sinatra/reloader'

get '/' do
  "<h1>Hi there!</h1>"
end

get '/:name' do
  "Good to see you, #{params[:name]}."
end

get '/:firstname/:lastname' do
  "Your name is #{params[:lastname]}. #{params[:firstname]} #{params[:lastname]}"
end
```

## You do: 99 Bottles of Beer

https://github.com/ga-dc/99_bottles_of_beer


#[Next: Views](views.md)

### Sample Quiz Questions

- List the 5 HTTP request methods. How do they relate to the 4 CRUD actions?
- What is the purpose of Sinatra's `layout.erb` file?
- Write an html `<script>` tag that links to a js file in `public/js/app.js`
- What's the difference between PATCH and PUT?
- What's the difference between `#{ }`, `<% %>`, and `<%= %>`?
