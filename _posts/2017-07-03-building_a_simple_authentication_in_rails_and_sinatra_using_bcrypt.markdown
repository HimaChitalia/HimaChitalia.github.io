---
layout: post
title:  "Building a Simple Authentication in Rails and Sinatra Using Bcrypt"
date:   2017-07-03 11:51:04 -0400
---


![Imgur](http://i.imgur.com/VV2dTu8.jpg)

LinkedIn was breached in 2012, Tumblr in 2013 and most recently MySpace in June 2016. If you had accounts at any of these sites, you may have been advised to change your password. But it doesn’t make a bitter tea any sweeter.
Why? Let’s see…

> According to [Ofcom’s Adults’ Media Use and Attitudes Report 2016](https://www.ofcom.org.uk/__data/assets/pdf_file/0026/80828/2016-adults-media-use-and-attitudes.pdf), Four in ten internet users say they tend to use the same passwords for most websites.

Just think about it. If user’s email with password gets leaked once, how many of his online accounts are in danger? That is what makes the security of your app the most crucial piece of it. First of all, I understand that you never want your application to get breached but as we need to consider all the possibilities while building an app, you never want hackers to see your user’s password. That’s where Password Encryption comes in.

Password Encryption means the translation of data into a secret code. It is the most effective way to achieve data security. For example, Your user’s simple password “`password1234`” may look like this when encrypted:

![Imgur](http://i.imgur.com/nFJtWvU.png)
Password encryption: example

This is not a rendom result. Everytime you encrypt the same password you will get the same result. Isn’t it amazing?
There are many ways to do this. But today, we will discuss the basic encryption system with Ruby Gem — [Bcrypt](https://rubygems.org/gems/bcrypt/versions/3.1.11) and Active Record method [has_secured_password](http://api.rubyonrails.org/classes/ActiveModel/SecurePassword/ClassMethods.html).

## Application Controller:

First of all, we need to make sure that our Application Controller is configured properly with helper methods looged_in? and current_user.

If you have Sinatra, your application_controller.rb file may look like this:
```
require ‘./config/environment’
class ApplicationController < Sinatra::Base
configure do
 enable :sessions
 set :session_secret, “coding_secret”
end
get '/' do 
  puts "Hello, user"
end
helpers do
def logged_in?
   !!current_user
  end
def current_user
   @current_user ||= User.find_by(id: session[:user_id]) if session[:user_id]
  end
end
end
```

If you have Rails app, your appliction_controller.rb file may look like this:

```
class ApplicationController < Sinatra::Base
configure do
 enable :sessions
 set :session_secret, “coding_secret”
end
def hello
 puts "Hello, user"
end
helpers
def logged_in?
   !!current_user
  end
def current_user
   @current_user ||= User.find_by(id: session[:user_id]) if session[:user_id]
  end
end
```
## Config.ru:
This configuration is only needed if you have a Sintra App. Add below code in your config.ru file:

```
use Rack::MethodOverride
use UserController
run ApplicationController
```

use Rack::MethodOverride: This line will let help browser send patch, put and delete http request to server.
Other two lines will help us run our controllers.

## Route.rb file:
This is the backbone file of Rails app. You need to add routes to your routes.rb file so that it can easily find the routes that your user is interested in. Add below routes to your config/routes.rb file:

```
get ‘/users/signup’ => ‘users#new’
post ‘/users/signup’ => ‘users#create’
get '/login' => 'sessions#new'
post '/login' => 'sessions#create'
get ‘users/logout’ => ‘users#destroy’
get '/' => ‘application#hello’
```

## Bcrypt Ruby Gem:

Add gem gem ‘bcrypt’ to your file and run bundle install in your terminal.

## User Migration:
You can add as many attributes as you may need to your user like username, email, date of birth, etcetera. But one of the important thing you need in your user migration file is a password_digest column.

If you have a Sinatra app, run command` rake db:create_migration NAME=create_users`

If you have a Rails app, run command `rails g migration User`

This should create a create_users.rb migration file for you:

```
class CreateUsers < ActiveRecord::Migration[5.1]
  def change
    create_table :users do |t|
      t.timestamps
    end
   end
end
```

Now, add password_digest and other necessary columns in it.

```
class CreateUsers < ActiveRecord::Migration[5.1]
  def change
    create_table :users do |t|
      t.string :username
      t.string :password_digest
      t.string :email
 
      t.timestamps
    end
  end
end
```

Now run `rake db:migrate` to add User table to database.
## User model:

If you already don’t have, create a file user.rb into your model directory with the command `touch user.rb.` Now add below code to your user.rb file:

```
class User < ActiveRecord::Base
has_secure_password
end
```

This magical Active Record method makes sure that password is present in user’s submission and also it matches with password confirmation. Here, is a [Github repo](https://github.com/rails/rails/blob/52ce6ece8c8f74064bb64e0a0b1ddd83092718e1/activemodel/lib/active_model/secure_password.rb) if you would like to dive in details.

As the name says, has_secured_password covers only your password. If you want even username and email to be unique, you need add custom validations for that. Update your user.rb file to look like this:

```
class User < ActiveRecord::Base
has_secure_password
validates :username, :email, presence: true, uniqueness: true
end
```

## User Controller:

Now, go to your controller folder and create a file users_controller.rb with the command touch users_controller.rb in your terminal.

If you have Sinatra app, add below code to your users_controller.rb file:

```
class UserController < ApplicationController
get ‘/signup’ do
  if !logged_in?
   @user = User.new
   erb :’users/signup’
  else
   redirect to ‘/tips’
  end
 end
post ‘/signup’ do
  @user = User.new(username: params[:username], password: params[:password], password_confirmation: params[:confirm_password], email: params[:email])
  if @user.save
   session[:user_id] = @user.id
   redirect to ‘/tips’
  else
  erb :’users/signup’
   end
 end
get ‘/login’ do
  if !logged_in?
   erb :’users/login’
  else
   redirect to ‘/’
  end
 end
post ‘/login’ do
  user = User.find_by(username: params[:username])
  if user && user.authenticate(params[:password])
   session[:user_id] = user.id
   redirect to '/'
  end
 end
get ‘/logout’ do
  if logged_in?
   session.clear
   redirect to ‘/login’
  else
   redirect to ‘/’
  end
 end
end
```

If you have a Rails app, add below code to your users_controller.rb file:

```
class UsersController < ApplicationController
def new 
  if !logged_in?
   @user = User.new
   render :’users/signup’
  else
   redirect to ‘/’
  end
 end
def create 
  @user = User.new(user_params)
  if @user.save
     session[:user_id] = @user.id
     redirect to ‘/’
  else
     render :’users/signup’
  end
end
private
def user_params
  params.require(:user).permit(:name, :email, :password, :password_confirmation)
end
end
```

## Sessions Controller:
If you have Rails app, you also need Sessions Controller. Go to your controller folder and run command` touch sessions_controller.rb` in your terminal to create a file. Now add below code to that method:

```
class SessionsController < ApplicationController
def new 
  
end
def create 
  @user = User.find_by(email: params[:email])
  if user && user.authenticate(params[:password])
     session[:user_id] = @user.id
     redirect to ‘/’
   else
     render :’users/signup’
   end
  end
end
def destroy 
  if logged_in?
   session.clear
   redirect to ‘/login’
  else
   redirect to ‘/’
  end
end
end
```

## Views:

To let user signup/ login, you will need to render a form.

If you have a Sintra app, add file login.erb and signup.erb in your app/views/users directory.

If you have a Rails app, add file login.html.erb and signup.html.erb in your app/views/users directory.

Your sign up form may look like this:

```
<h2>Sign Up for the Coding Tips </h2></br> </br>
 <form action=”/signup” method=”POST”>
 <p> Username: <input type=”text” name=”username” value=”<%= @user.username %>”></p>
 <p> Email: <input type=”email” name=”email” value=”<%= @user.email %>”></p>
 <p> Password: <input type=”password” name=”password”></p>
 <p> Confirm password: <input type=”password” name=”confirm_password”></p></br></br>
 <input type=”submit” value=”Sign Up”>
 </form>
Your login form may look like this:
<h2 >Login to the Coding Tips</h2></br></br>
 <form action=”/login” method=”POST”>
 <p> Email: <input type=”text” name=”email”></p>
 <p> Password: <input type=”password” name=”password” ></p></br></br>
 <input type=”submit” value=”login”>
 </form>
```
This is it! Now, you are all ready to try your own authentication system. You have built it from scratch. So, be proud of yourself.

I hope this tutorial helps you out in your Ruby journey. Any suggestion or question, please feel free to write me at hima.chhag@gmail.com.

Happy Coding!


