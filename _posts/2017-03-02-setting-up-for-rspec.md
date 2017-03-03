---
layout: post
title: $ rails new with RSpec
date: 2017-03-02 15:00:00
description: Quick reference for setting up RSpec and Capybara in a new Rails app.
published: true
---
This is a quick rundown of how to install RSpec and Capybara in a new Rails project.
<hr>
<br>
Create a new rails app without Test::Unit
{% highlight ruby %}
$ rails new [app_name] --skip-test-unit
{% endhighlight %}

Add RSpec and Capybara to the Gemfile
{% highlight ruby %}
# Gemfile
group :development, :test do
	gem 'rspec-rails'
end
group :development do
	gem 'capybara'
end
{% endhighlight %}

Install gems
{% highlight ruby %}
$ bundle install
{% endhighlight %}

Generate spec file directory
{% highlight ruby %}
$ rails generate rspec:install
{% endhighlight %}

Add a directory for feature/integration specs
{% highlight ruby %}
$ mkdir spec/features
{% endhighlight %}
