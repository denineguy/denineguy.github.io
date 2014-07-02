---
layout: post
title: "password generating app with sinatra"
date: 2014-07-02 00:08:45 -0400
comments: true
categories: rails sinatra, ruby
languages: ruby
---
##Web Applications with Sinatra

Over the past week, at The Flatiron School we have been learning about Rack and creating web applications using Sinatra.  Sinatra is a free open source software web application library, that is a domain-specific language written in Ruby, and is dependent on the Rack web service interface.

Unlike Ruby on Rails, Sinatra does not follow the typical model-view-controller pattern.  Instead it focuses on quickly creating web applications in Ruby with minimal effort.  After further reading, I discovered that Sinatra is great for creating small apps and is ideal if your controller structure can fit into one or two pages of code. With this in mind, I decided to set it out and try to create my own app.

## My Hatred for Passwords
I love the internet, always have and always will, but there is one thing in particular that has become increasingly annoying to me and that is the need for passwords on nearly every site.  I mean it has gotten so bad that some companies don’t even let you peruse their site to figure out what they are all about without having to sign in. We know nothing about them but to learn more they have to capture data on us.  I personally think this should be illegal but that is neither here not there.  The reality of the situation is that I can’t keep up with these passwords anymore.  It was so bad at one point that I literally had the same password for every site.  YES I know that is bad, but I just couldn’t think of any better passwords.  I have since started changing them, but am running out of ideas for passwords.  Since it doesn’t seem like the need for passwords on websites are going away anytime soon, I decided to see if I could create a random password generator for myself...  that is until I figure out a better technology to eliminate the need for passwords altogether.

Please note that my password generator does not produce the most secure passwords at this time.  This is a work in progress and I will continue to develop this app.

## Word-Generator Form
```html
<form action='/password' method="POST">
<h3>Please enter two words that are at least 5 letters long</h3>
  <p>
    <label for="first_word">First Word:</label>
    <input id="first_word" type="text" name="password[first_word]"> </br> 
    <label for="second_word">Second Word:</label>
    <input id="second_word" type="text" name="password[second_word]">
  </p>
  <input type="submit" value="submit"/>
</form>
```

First things I didn’t want an extremly abstract password that I wouldn’t remember.  I thought if I set two words that I could manipulate, maybe I will rememeber them a little better. Above is a form that would take in two words of my choice.

## Class Password 
```ruby
require 'sinatra'
class Password

  attr_accessor :first_word, :second_word, :password_generator

  def initialize
    @generator = []
  end
  
  def word1 
    word_split = first_word.split("")
    scramble = word_split.permutation(rand(2..4)).to_a[rand(30)]
  end

  def word2
    word_split = second_word.split("")
    scramble = word_split.permutation(rand(2..3)).to_a[rand(20)]
  end  

  def password_generator
    @generator << [word1, rand(10), word2, "_", rand(20)].join
    @generator[0]
  end

end
```
I then created a Password Class that takes in two words and permutates them. I randomly select a permutation of 2-4 characters for the first word out of several different permutations, and 1-3 characters for the second word.   I then join the two words with random numbers, and a non-word character, which will ultimately generate my password.


##App.rb - Routes

```ruby
module PasswordSinatra
  class App < Sinatra::Base

    get '/' do
      erb :index
    end

    get '/word_generator' do
       erb :word_generator 
    end

    post '/password' do
      @password = Password.new
      @password.first_word = params[:password][:first_word]
      @password.second_word = params[:password][:second_word]
      @password.password_generator = params[:password][:password_generator]
  
      erb :password
    end

    get '/password' do
      erb :word_generator
    end

  end
end
```
This code above shows my routes, the HTTP method (GET or POST) with the corresponding URL destination when a request is made to the server.  The root goes to my index page.  When the link on the index page is clicked it takes the user to a word generator form page, where the user has to enter two words and click submit. This will ultimately end up on the password page which will show them their newly created password. If a user does not like that password they can either refresh page to generate a new password permutation or go back to the word_generator from and submit two new words to create an entirely new password. 

##Screenshots of the Password Generator Application

####Index Page 
![index page](/images/index_page.png "Password Generator Index Page")


####Word Generator Form Page
![word generator form page](/images/word_generator.png "Word Generator Form Page")


####Password Page  
![password generator page](/images/password_generator.png "Password Generator Page")

Voila and there it is my very own randomly generated password. <em> pp7sar_10 </em>

Well this is my app.  I will continue to improve this app and hope to have it deployed shortly.

###References:
Here is a great blog post to learn when to Rails vs Sinatra: [Rails or Sinatra: The Best of Both Worlds](http://www.sitepoint.com/rails-or-sinatra-the-best-of-both-worlds/ "Title"). 
