---
layout: post
title: Weather API and Sandy Metz
date: 2017-06-17 20:43:00
description: My git log into SOLID design
published: true
---

<pre><h1>d7fe119 initial commit</h1></pre>
# "Writing tests isn't enough..." 
<br>
Damn. Sandy Metz's [famous talk on SOLID design](https://www.youtube.com/watch?v=8STtzjyDTTQ) in Ruby, along with the unparalleled [POODR](http://www.poodr.com/), has left me wanting to become a better developer. So, I sit down at my text editor, and I... I... well, what do I do? I need to design, right? I can't simply spit out perfect code. I need to design my tests, too! I don't want to write bad code because now I *know* better.
<br><br>
I sat on my current project, which I'll get into in a moment, for days. It's gone through many many versions, however, after all that work, my `git log` shows up empty. I've written nothing. This may sound like a good thing, taking careful steps to plan before attacking this new challenge, but a whole lot of nothing got done. I became paralyzed by the thought of writing bad code.
<br><br>
The more I learned, the more I learned I needed to learn. And I fell into the trap that Sandy mentions, the trap of over-designing based on what we *think* will happen in the future. So, my solution is to go through the projects iterations here, and welcome in the *process* of design by trudging through stanky code.
<br><br>
# Project Overview
<br>
The plan is to build an API wrapper for [Dark Sky's Weather API](https://darksky.net/poweredby/). I'll use this wrapper to build out a [Telegram](https://core.telegram.org/bots/) chat bot that will let you know whether or not you'll need an umbrella. Simple, right? Yes. But not easy. This article will only focus on the API, the bot will have to come later. Notable gems include [HTTParty](https://github.com/jnunemaker/httparty), and [RSpec](https://github.com/rspec/rspec) with [VCR](https://github.com/vcr/vcr) and [WebMock](https://github.com/bblimke/webmock). 
<br><br><br><br>
<pre><h1>cb883c6 added first spec</h1></pre>
Without much planning, heres what I know based off of [Dark Sky's API docs](https://darksky.net/dev/docs). There are two endpoints: a [Forecast Request](https://darksky.net/dev/docs/forecast) which returns the current weather forecast, and a [Time Machine Request](https://darksky.net/dev/docs/time-machine) which returns the observed or forecast weather conditions for a date in the past or future. I'm going to be using the Forecast endpoint.
<br><br>
The endpoint returns a JSON object-- perfect. In order to make a request, I'm required to include three things: An API Key and the location's latitude and longitude. The resulting query path looks like this: `https://api.darksky.net/forecast/[key]/[latitude],[longitude]`
<br><br>
Looking at HTTParty's docs, I see that all I need to do to send a request is supply the url to `HTTParty.get`, and I'll get back an `HTTParty` object, which includes methods like `#body`, `#headers`, and `#ok?`. So I can write my first specs in true TDD fashion. (Note that I'm using RSpec with VCR and WebMock).
<br><br>
{% highlight ruby %}
require './spec_helper'

RSpec.describe 'dark sky', :vcr do
  context 'when making a valid API request' do
    it 'returns with a status of OK' do
      VCR.use_cassette('new_york') do
        expect(forecast).to be_ok
      end
    end
  end
end
{% endhighlight %}
<br>
So now, I have to write a method, `forecast`, which returns an object that responds to `#ok?`, which, as we discussed, an `HTTParty` object does!
<br><br>
{% highlight ruby %}
require 'httparty'

def forecast
  longitude = '40.7128' #NYC
  latitude = '74.0059'	#NYC
  HTTParty.get("https://api.darksky.net/forecast/#{DARKSKY_API_KEY}/#{latitude},#{longitude}")
end
{% endhighlight %}
<br>
As far as API wrappers go, this one isn't so bad. Thanks to HTTParty and the `HTTParty` object that is returns, making API calls is really that simple. However, let's not pretend, this code sucks. It's more like a script; something that you may just have to run once and then burn it. However, our scope is larger than that. We want to create a reusable class. So let me do some refactoring and see if I can't set myself up for creating a class with a single responsibility.
<br><br><br><br>
<pre><h1>898006e created DarkSky class</h1></pre>
{% highlight ruby %}
require 'httparty'

# Wrapper for Dark Sky's Forecast API
class DarkSky
  include HTTParty
  base_uri 'https://api.darksky.net/forecast'

  def initialize(latitude, longitude)
    @latitude = latitude
    @longitude = longitude
  end

  def forecast
    DarkSky.get("/#{DARKSKY_API_KEY}/#{@latitude},#{@longitude}")
  end
end
{% endhighlight %}
<br>
Now this feels a lot more satisfying, doesn't it! We can initialize the class with the coordinates of a location, such as `new_york = DarkSky.new('40.7128', '74.0059')`, and then call an intuitive `new_york.forecast`. It makes the test writing a bit clearer too:
{% highlight ruby %}
require './spec_helper'

RSpec.describe 'dark sky', :vcr do
  let(:new_york) { DarkSky.new('40.7128', '74.0059') }

  context 'when making a valid API request' do
    it 'returns with a status of OK' do
      VCR.use_cassette('new_york') do
        expect(new_york.forecast).to be_ok
      end
    end
  end
end
{% endhighlight %}
<br>
So how do we know if our new class here has a single responsibility? Let's see what Sandy has to say in POODR: 

>Another way to hone in on what a class is actually doing is to attempt to describe it in one sentence. Remember that a class should do the smallest possible useful thing. That thing ought to be simple to describe. If the simplest description you can devise uses the word “and,” the class likely has more than one responsibility. If it uses the word “or,” then the class has more than one responsibility and they aren’t even very related. -[POODR](http://www.poodr.com/) pg 22

<br>
So what does our class do in one sentence? "It returns an HTTParty object from an API request to an external API."
<br><br>
I'm still getting code smells. However, I'm not sure where to go now. Sandy tells me that I should make design decisions prematurely. That I should resist, even if I fear my code would "dismay the design gurus." So what do I do? I write code that embraces change.
<br><br><br>
<pre><h1>8383ac6 embraced change</h1></pre>
{% highlight ruby %}
require 'httparty'

# Wrapper for Dark Sky's Forecast API
class DarkSky
 attr_accessor :latitude, :longitude

  include HTTParty
  base_uri 'https://api.darksky.net/forecast'

  def initialize(latitude, longitude)
    @latitude = latitude
    @longitude = longitude
  end

  def forecast
    DarkSky.get(build_path)
  end

  def build_path
   "/#{DARKSKY_API_KEY}/#{latitude},#{longitude}"
  end
end
{% endhighlight %}
<br>
Ahhh... That's better. But why did I make these design choices now, even though I don't know where I'm going? Let's first talk about `latitude` and `longitude`.
<br><br>
Right now, those variables simply contain data. However, thanks to `attr_accessor`, that data is now accessed by sending a message; meaning, if `latitude` or `longitude` ever need to change, I now only have to redefine the methods. It seems moot at this point because I'm initializing the variable when I initialize an object, but that too could need to change later. Sandy says:
> Data very often has behavior that you don’t yet know about. Send messages to access variables, even if you think of them as data. -[POODR](http://www.poodr.com/) pg 26

So that's what I did here.
<br><br>
Secondly, I moved the string interpolating for the API request path from `forecast`, into it's own method, `build_path`. Why? For one, the line was too long. For two, that `build_path` method is guaranteed to change later! (I'm going to want to be able to request some of the optional parameters that Dark Sky gives me). And for three, Sandy wants me to. 

> Methods, like classes, should have a single responsibility. All of the same reasons apply; having just one responsibility makes them easy to change and easy to reuse. All the same design techniques work; ask them questions about what they do and try to describe their responsibilities in a single sentence. -[POODR](http://www.poodr.com/) pg 29

<br><br><br><br>
<pre><h1>3843bee isolated responsibilities</h1></pre>
Something still bothers me about this code. Specifically, the `latitude` and `longitude`. These variables seem like they should belong to a separate object; maybe a `Location` class? However, I don't really have a good reason to build a new class yet.
<br><br>
Let's consider the examples: what if I'll want to store some coordinates as canonical data, i.e. `new_york = Location.new('40.7128', '74.0059')` and `los_angeles = Location.new('34.0522', '118.2437')` Surely I'd want a `Location` object for that. Or maybe I'd want to add methods to my objects like `Location#season`, `Location#local_time`, or `Location#elevation`. In this case it would make sense to me to abstract these methods out of our `DarkSky` class. 
<br><br>
However, as of now, I don't have those feature requests or specs, I just have an inkling. Premature design can be dangerous. It can trap me into building something that's more complicated than I need it to be, or at best, it can funnel my creativity by forcing me to only think of my application in one specific way. Sandy says: 
>Any decision you make in advance of an explicit requirement is just a guess. Don’t decide; preserve your ability to make a decision later. -[POODR](http://www.poodr.com/) pg 32

She also has a solution. I can move the handling of `latitude` and `longitude` into an anonymous class, i.e., a Ruby `Struct`.
{% highlight ruby %}
require 'httparty'

# Wrapper for Dark Sky's Forecast API
class DarkSky
	attr_accessor :location

  include HTTParty
  base_uri 'https://api.darksky.net/forecast'

  def initialize(latitude, longitude)
    @location = Location.new(latitude, longitude)
  end

  def forecast
    DarkSky.get(build_path)
  end

  Location = Struct.new(:latitude, :longitude)

  def build_path
  	"/#{DARKSKY_API_KEY}/#{location.latitude},#{location.longitude}"
  end
end
{% endhighlight %}
<br>
By making this seemingly gratuitous change now, I can easily extract `Location` into a separate class if, and only if, future specifications dictate.

<br><br><br><br>
<pre><h1>3ae321dd managed dependencies</h1></pre>
So far, I only have one class, so luckily there isn't any object coupling to worry about. (I'm not going to consider my class' integration with the HTTParty library.) But, I do need to think about how this class will be used; how it's methods will be called.
<br><br>
As it sits, any caller who wants to instantiate a `DarkSky` object will need to include `latitude` and `longitude` as parameters to the `DarkSky#new` method, in that order. That's not a tall order, but consider how easy it would be to mix those two up! Also, what if we pass in `Float`s, rather than `String`s? Even besides that, we're going to want to be able to add parameters later, without breaking any current calls to `DarkSky#new`! I'll want to take care of this now by allowing the caller to pass in a hash and providing defaults in case they're not included. 
{% highlight ruby %}
require 'httparty'

# Wrapper for Dark Sky's Forecast API
class DarkSky
  attr_accessor :location

  include HTTParty
  base_uri 'https://api.darksky.net/forecast'

  def initialize(args = {})
    latitude = args.fetch(:latitude, '40.7128').to_s
    longitude = args.fetch(:longitude, '74.0059').to_s
    @location = Location.new(latitude, longitude)
  end

  def forecast
    DarkSky.get(build_path)
  end

  Location = Struct.new(:latitude, :longitude)

  def build_path
    "/#{DARKSKY_API_KEY}/#{location.latitude},#{location.longitude}"
  end
end
{% endhighlight %}

<br><br><br><br>
<pre><h1>2131a7e stored response in instance variable</h1></pre>
One last thing before I wrap up this section of my DarkSky class: making responsible requests. When I begin thinking about how I'm going to parse the JSON response from Dark Sky, I begin thinking about requests too. Based on the [Dark Sky's API docs](https://darksky.net/dev/docs) the `Forecast` endpoint has an optional `exclude` query string, which will allow me to make a query, and return a limited response. So, if I only want the `currently` response block, I can exclude all the others. This can be useful for when I want to only parse that block. However, this can lead me to make multiple calls to the API endpoint-- every time I want want to parse a specific piece of data.
<br><br>
My solution then is to retrieve the entire response, and then parse things on my end. To do this, I'll store the response in an instance variable, and only make the request if no response has been stored in that variable yet. 
{% highlight ruby %}
require 'httparty'

# Wrapper for Dark Sky's Forecast API
class DarkSky
  attr_accessor :location, :response

  include HTTParty
  base_uri 'https://api.darksky.net/forecast'

  def initialize(args = {})
    latitude = args.fetch(:latitude, '40.7128').to_s
    longitude = args.fetch(:longitude, '74.0059').to_s
    @location = Location.new(latitude, longitude)
  end

  def forecast
    @response ||= DarkSky.get(build_path)
  end

  Location = Struct.new(:latitude, :longitude)

  def build_path
    "/#{DARKSKY_API_KEY}/#{location.latitude},#{location.longitude}"
  end
end
{% endhighlight %}

<br>
There is a caveat with this solution, however. What happens when I want to parse a stored response that I made hours ago? That data would be stale! I could write a job to clear the `@response` variable, but for my purposes, I'll instead leave that responsibility to the caller. Not a great design feature, but an appropriate solution for now. Because I made `@response` `attr_accessible`, I can call something like `new_york.response = nil`, and force the class to make a new request. 

<br><br><br><br>
<pre><h1>a822ae1 [WIP] To Be Continued</h1></pre>
Thanks to only the first two chapters of [POODR](http://www.poodr.com/), I feel more confident beginning new projects. Like TDD did for me before, the practice of refactoring has freed me up from needed to feel like I need to have all the answers right now. This code may not be perfect, but it's steps closer to matching my current level of understanding object oriented design.
<br><br>
The next steps for my newly built `DarkSky` class is to write some more specs and to parse the data returned. `HTTParty` has a great `#parsed_response` method that will turn the body of JSON into a hash. This data structure will provide a ton of dependency issues for me to tackle next. I'm sure Sandy will be there to help!
