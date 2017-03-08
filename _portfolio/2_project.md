---
layout: post
title: Secret Board
description: A Study of Auth, Sessions, and BDD/TDD
published: true
img: /img/anonymous.jpg
---
<h1>
<a href="https://young-stream-65922.herokuapp.com" target="_blank">Deployed Here</a> | 
<a href="https://github.com/Thomascountz/secret_board/" target="_blank">Github Repo</a> 
</h1>
<br>
<h1>What is this?</h1>
<br>

<p>
An unstyled Rails app that I wrote while working through <a href="http://www.theodinproject.com" target="none">The Odin Project</a>.
</p>

<br>
<h1>What problem does it solve?</h1>
<br>

<p>
Secret Board allows non-signed-in users who visit the site to see a list of anonymous text-based posts. When a user logs-in, that user can see the same posts and their respective authors, as well as create a new post themselves. That new post will then be seen as anonymously authored to non-logged-in users.
</p>

<br>
<h1>What's inside?</h1>
<br>
<p>
<strong>tl;dr</strong> As my first solo rails project, I practiced wireframing, TDD with RSpec and Capybara, and built a focused MVP that will allow me to feel more confident using industry standard authentication gems.
<hr>
<br>
This project was an obvious segue from <a href="https://www.railstutorial.org" target="_blank">Michael Hartl's Tutorial</a>, but I wanted to do this one on my own. What I had most trouble following in Hartl's book was his testing, which as of the Fourth Edition (Rails 5), uses Test::Unit, rather than RSpec, which I'm more familiar with. This led me to want to try my hand at writing my own tests with RSpec, rather than simply adapting his tests for my needs.
<br><br>
Thanks to <a href="http://www.justinweiss.com" target="_blank">Justin Weiss</a>’ free 7-Part Email Course, I felt like I could wrap my head around TDD/BDD. I knew about writing tests and had some practice with it, but true TDD was something I struggled with with Rails. With so many tests to write that cover so many things, where do you start? 

<div class="img_row" style="height: 100%">
	<img class="col three" src="/img/testing_pyramid.png">
</div>
<div class="col three caption">
	Testing Pyramid from from Justin Weiss. 
</div>

This Pyramid, combined with the opening chapters of Sam Ruby's <a href="https://pragprog.com/book/rails5/agile-web-development-with-rails-5" target="_blank">Agile Web Development with Rails 5</a>, I felt like I had the tools I needed to feel confident.

I started with sketches and user stories. Just me and a yellow pad, I sketched out what my UI would look like and some simple flow logic. 

<div class="img_row" style="height: 100%">
	<img class="col three" src="/img/secret_board_wireframe.jpg">
</div>
<div class="col three caption">
	It's not pretty, but it focused me in on an achievable MVP 
</div>

As I'll find out later, I forgot one thing in the wireframe: The <code>login</code> link on the <code>Posts</code> page should change to <code>logout</code> when a user is logged in.
<br>
<br>
<p>
Next, I wrote some user stories:
<ul>
  <li>When I visit the site, I see a list of text-based posts with anonymous authors and a link to log in</li>
  <li>When I click 'login', I see a form with two inputs for an email address and password, and a submit button</li>
  <li>When I type invalid information and click 'submit', I see text explaining that there is an error</li>
  <li>When I type valid information and click 'submit', I'm taken back to the homepage</li>
  <li>On the homepage, I see text telling me that I'm logged in</li>
  <li>When I refresh the page, the text is gone</li>
  <li>On the homepage, I see the same text-based posts, this time with named authors and a link to logout</li>
  <li>On the homepage, I also now see a link to create a new post</li>
  <li>When I click 'create post' I see a form with one input to create the body of a post, and a submit button</li>
  <li>When I leave the box blank and press submit, I see text explaining that there is an error</li>
  <li>When I add text to the input and press submit, I'm taken back to the homepage</li>
  <li>On the homepage, I see text telling me that the post has been created successfully</li>
  <li>When I refresh the page, the text is gone</li>
  <li>On the homepage, I see a new post which is authored by me</li>
  <li>When I click 'logout', I'm still on the homepage</li>
  <li>On the homepage, I still see the post I created but now authored by anonymous</li>
</ul>
</p>
<br>
With a wireframe in hand and a list of user stories, it was relatively simple to come up with some integration tests, which obviously failed to begin with.
<br>
<br>
{% highlight ruby %}
# secret_board/spec/features/post_index_spec.rb

...
  
  context 'when user is anonymous' do
  
  ...
  
    it 'renders a list of posts'
    it 'has a link to login' 
    it 'does not render the name of the post author' 
  end
  
  context 'when user is signed in' do
  
  ...
    
    it 'renders the name of the post author' 
    it 'has a link to create a new post' 
    it 'adds a new post'
    it 'has a link to logout' 
  end
  
  private
  
    def log_in_as(user)
    ...
    end
    
...

# secret_board/spec/features/user_login_spec.rb
...
                           
  context "with a user email that doesn't exist" do
    it 'rerenders the form with an alert message' 
  end
  
  context "with an incorrect password" do
    it 'rerenders the form with an alert message' 
  end
  
  context "with valid login information followed by logout" do
    it 'rerenders the form with an alert message' 
  end
  
...
{% endhighlight %}
<br>
Led by these tests, I had to figure out what kind of controllers and models I would need, and then then begin to write tests for those too. I didn't complete the entire test suite before I began coding anything at all, but because I was familiar with the sessions-login pattern, thanks to Hartl's book, I was able to gently guide myself through the TDD process.
<br>
<br>
Continuing through the process, I had my wireframe, user stories, and integration specs guiding me to a well-tested MVP. That last part is what was most important to me. It was tempting to add features here and there, but the development process stayed on track thanks to the groundwork that was laid before I even typed <code>$ rails new</code>.
</p>

<br>
<h1>Where do we go from here?</h1>
<br>

<p>
This was a very powerful app for me to create. With complete test coverage, I can now implement numerous user features that one might expect from an app that solves this problem, namely:
<ul>
  <li>Account Creation/Deletion</li>
  <li>"Remember me" on login</li>
  <li>Edit/Delete posts</li>
  <li>Styling and Responsiveness</li>
  <li>Moderation/Admin Accounts</li>
</ul>

This toy app is also exactly the kind of thing I'll use to test upgrading DIY-Auth to the standard auth solution, <a href="https://github.com/plataformatec/devise" target="_blank">Devise</a>, and plan on doing just that in this app's second iteration. 
</p>

<br>
<h1>What did I learn?</h1>
<br>
<p>
Well in case I haven't already covered everything, I began understanding some agile methodologies while reinforcing my understanding of RSpec and Capybara. Most obviously, I began to understand browser sessions, model validations, controller before-actions, and debugging.
</p> 