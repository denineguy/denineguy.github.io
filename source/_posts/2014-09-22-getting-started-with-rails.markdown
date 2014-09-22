---
layout: post
title: "Getting Started with Rails"
date: 2014-09-22 10:19:58 -0400
comments: true
categories: ruby rails
---

[Rails](http://guides.rubyonrails.org/) has rapidly become one of the most powerful frameworks for building dynamic web applications and it is written in the Ruby language. Today there are several startups that use Rails including GitHub, Shopify, Hulu,Quirky, Airbnb. There are also several web development companies that focus on Rails development, among those are Pivotal Labs and Thoughbot. One of the great things about Rails is that it is 100% open-source and as a result cost nothing to download.  The framework is designed to make building web applications easier by making assumptions about what every developer needs to get started and it allows you to write less code while accomplishing more than other languages and frameworks.
<!-- more -->

Rails follows two major guiding principles: 

1. DRY(Don't Repeat Yourself) - By not repeating the same code over and over again the code is more maintainable, extensibility and minimizes bugs
2. Convention over Configuration - Rails believe it has created the best ways to do many things in a web application and this it follow many conventions rather than having the developer set up many configuration files.

##Let's get Started
We are going to create a simple Blog Post. The first thing you need to do is set up your Rails environment.  You should have the following installed:

1. [Ruby language](http://www.ocf.berkeley.edu/~kelu/interviews/questions.html)
2. [Ruby Gem's packaging system](http://rubygems.org/)
3. [SQLite3 Database](http://www.sqlite.org/)

Assuming that all of those have been downloaded successfully, you now need to install Rails. To install Rails you just type the following in your command line
```
  $ gem install rails
```
To ensure it is installed correctly run the following:
```
  $ bin/rails --version
```
The version should be 4.1.2 or greater

Once your environment is completely setup it is easy to set up a new Rails app. You would enter rails new and the name of the application in the command line.  Enter the following in your command line:

```
  $ rails new blog
```
Rails will automatically run bundler for you, but go to your Gemfile and add any other Gems you wish to use.  After you have added them to your file run ``` bundle install ``` in the command line. Now cd into your new application and open it in your text editor.  You will see that Rails has auto-generated a number of files and folders for you. To get a full explanation and purpose for each file and folder checkout the [Rails Guides](http://guides.rubyonrails.org/getting_started.html) for full documentation on it.

## Model-View-Controller
Rails follows the [model-view-controller(MVC)](http://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) pattern, which separates the applicaton behavior(business logic) from the input and the user interface. In our case business logic is the model for posts and comments while the user interface is the web page in the browser.

In essence for Rails applications when a browser sends a request it is received by the web server and then passed to the Rails controller, which determines what to do next.  In some cases the controller will render a view, an HTML template that gets sent back to the server.  In most cases, however, the controller will interact with the model, a Ruby object which is a component of the site(such as a post) and it will communicate with the database. After communicating with the database the controller will step in and render a view, the web page in HTML format.

![Rails MVC Diagram](/images/mvc_image.png "A detailed diagram of MVC in Rails")

##Models
To get started we need to think about what we need for our application.  In our case we are creating a very simple blog and at a minimum we need to think about the posts for the blog and the comments. The posts should have a title and content.  The comment should have a commenter and content and post_id which is a foreign key to posts and this bascially states that the comment ```belongs_to``` a post.  In order to create your model you will write the following in your command line.

``` 
  rails generate model Post title:string content:text
  rails generate model Comment content:text post_id:integer
```

Somethings to note is that model is typically singular. After you write the name of the model(your object) you define the attributes of the object by writing it in the format attribute:type, where attribute is the title and type would be string, text, integer, boolean, date. The difference between string and text is that a string is for short text input usually 255 characters or less and text is user for more input like content which could potentially be more than 255 characters. After generating the models the following should be populated.

```
♥ rails generate model Post title:string content:text
    invoke  active_record
    create    db/migrate/20140920155246_create_posts.rb
    create    app/models/post.rb
    invoke    test_unit
    create      test/models/post_test.rb
    create      test/fixtures/posts.yml
[11:52:47] blog
♥ rails generate model Comment content:text post_id:integer
    invoke  active_record
    create    db/migrate/20140920155332_create_comments.rb
    create    app/models/comment.rb
    invoke    test_unit
    create      test/models/comment_test.rb
    create      test/fixtures/comments.yml
```

The model is responsible for creating your post and comment models as well as your migration files for both models. Let's look at the migrations.  Go to the ```db/migrate``` folder and you will see migration files for both ```create_post.rb``` and ```create_comments.rb``` with a timestamp in front of the name of the file. These files create your Active Record migration and basically setup the structure for the database table which are essentially the attributes for each model. You will see that Rails automatically adds a timestamp which will generate the created_at and updated_at fields in your database and tell you when a record has been added or updated to the database.

```ruby Post.rb
class CreatePosts < ActiveRecord::Migration
  def change
    create_table :posts do |t|
      t.string :title
      t.text :content

      t.timestamps
    end
  end
end    
```

```ruby Comment.rb
class CreateComments < ActiveRecord::Migration
  def change
    create_table :comments do |t|
      t.string :commenter
      t.text :content
      t.integer :post_id

      t.timestamps
    end
  end
end

```

rails generate model, also creates your model files. If you go to your text editor in ```app/models``` you should now see a ```post.rb``` and ```comment.rb``` file. Both should look you have a class for the specific modes which inherits from ActiveRecord::Base.  In our models we need to tell Active Record how the models are associated with each other. To learn more about associations visit [Rails Active Record Association](http://guides.rubyonrails.org/association_basics.html).  This is how our foreign key will essentially work.  So let's think about this.  For any one post it is possible to have a lot of comments and a comment belongs to only one post.  We also want to make sure that a post has at least a title in order for it to be saved and a comment needs to have some content. We can do that by validating the presence of those attributes. Validations are covered in detail in [Rails Active Record Validations](http://guides.rubyonrails.org/active_record_validations.html).  So let's write the following associations in our model.

```ruby Post.rb
class Post < ActiveRecord::Base
  has_many :comments
  validates_presence_of :title 
end
```

```ruby Comment.rb
class Comment < ActiveRecord::Base
  belongs_to :post
  validates_presence_of :content
end
```
Now that we have our models and table configuration set up we now need to migrate the table to actually generate the database table.  In you command line run ```rake db:migrate```
You should see that a create_table post and create_table comment were both generated. Visit [Rails Database Migrations](http://guides.rubyonrails.org/migrations.html) for more information on Migrations

```ruby Rake db:migrate
♥ rake db:migrate
== 20140920155246 CreatePosts: migrating ======================================
-- create_table(:posts)
   -> 0.0023s
== 20140920155246 CreatePosts: migrated (0.0024s) =============================

== 20140920155332 CreateComments: migrating ===================================
-- create_table(:comments)
   -> 0.0012s
== 20140920155332 CreateComments: migrated (0.0013s) ==========================
```

Once your migration finishes a ```schema.rb``` will be generated. Go to you text editor and go to the folder ```db\schema.rb```This will show the layout of your tables, which should look like the following.

```ruby Schema.rb
ActiveRecord::Schema.define(version: 20140920155332) do

  create_table "comments", force: true do |t|
    t.string   "commenter"
    t.text     "content"
    t.integer  "post_id"
    t.datetime "created_at"
    t.datetime "updated_at"
  end

  create_table "posts", force: true do |t|
    t.string   "title"
    t.text     "content"
    t.datetime "created_at"
    t.datetime "updated_at"
  end

end
```
Now let's see what our website looks like so far.  In the command line start your rails server by entering ```rails s```. Once that runs enter ```http://localhost:3000``` in your browser and you should see the following. 

![Rails Default Information Page](/images/welcome_aboard_rails.png "Rails Default Information Page Welcome Aboard.")

In the next section we will begin to render our application in the browser

##Controllers
In order to begin to render the application on the page you need at least a controller and a view. For our application we will need to create a Post Controller and a Comments Controller. 
As we discussed previously the controllers purpose is to recieve specific request from the application.  The routers purpose is to decide which controller recieves which request.  Each action's purpose is to collect  information to provide it to a view. A view's purpose is to display this information in a human readable format. View templates are written in a language called eRuby (Embedded Ruby).

To create your controllers you need to enter ```rails g controller Posts``` and ```rails g controller Comments```.  Notice that unlike your models which are singular your controllers are plural. 

``` ruby PostsController and CommentsController
♥ rails g controller Posts
      create  app/controllers/posts_controller.rb
      invoke  erb
      create    app/views/posts
      invoke  test_unit
      create    test/controllers/posts_controller_test.rb
      invoke  helper
      create    app/helpers/posts_helper.rb
      invoke    test_unit
      create      test/helpers/posts_helper_test.rb
      invoke  assets
      invoke    coffee
      create      app/assets/javascripts/posts.js.coffee
      invoke    scss
      create      app/assets/stylesheets/posts.css.scss
[11:25:23] (master) blog
♥ rails g controller Comments
      create  app/controllers/comments_controller.rb
      invoke  erb
      create    app/views/comments
      invoke  test_unit
      create    test/controllers/comments_controller_test.rb
      invoke  helper
      create    app/helpers/comments_helper.rb
      invoke    test_unit
      create      test/helpers/comments_helper_test.rb
      invoke  assets
      invoke    coffee
      create      app/assets/javascripts/comments.js.coffee
      invoke    scss
      create      app/assets/stylesheets/comments.css.scss
```      
Generating controllers will create both your controller files and your view files.  If you go to your text editor and go to app/controllers you should now see a ```comments_controller.rb``` and ```posts_controller.rb``` file, which both inherit from the Application Controller. If you go to app\views you will now see folders for comments and posts.

```ruby posts_controller.rb
class PostsController < ApplicationController
end
```

```ruby comments_controller.rb
class CommentsController < ApplicationController
end
```

##Routes
We will now add routes to our application.  Please enter the following in your ```routes.rb``` file.

```ruby Routes.rb
Rails.application.routes.draw do
  resources :posts do
    resources :comments
  end
end
```
This creates comments as a nested resource within posts. This is another part of capturing the hierarchical relationship that exists between posts and comments. This basically states that comments will only be viewed through routes generated for posts.

Now you can enter ```rake routes``` and you should see the following.
```ruby Rake Routes
♥ rake routes
           Prefix Verb   URI Pattern                                 Controller#Action
    post_comments GET    /posts/:post_id/comments(.:format)          comments#index
                  POST   /posts/:post_id/comments(.:format)          comments#create
 new_post_comment GET    /posts/:post_id/comments/new(.:format)      comments#new
edit_post_comment GET    /posts/:post_id/comments/:id/edit(.:format) comments#edit
     post_comment GET    /posts/:post_id/comments/:id(.:format)      comments#show
                  PATCH  /posts/:post_id/comments/:id(.:format)      comments#update
                  PUT    /posts/:post_id/comments/:id(.:format)      comments#update
                  DELETE /posts/:post_id/comments/:id(.:format)      comments#destroy
            posts GET    /posts(.:format)                            posts#index
                  POST   /posts(.:format)                            posts#create
         new_post GET    /posts/new(.:format)                        posts#new
        edit_post GET    /posts/:id/edit(.:format)                   posts#edit
             post GET    /posts/:id(.:format)                        posts#show
                  PATCH  /posts/:id(.:format)                        posts#update
                  PUT    /posts/:id(.:format)                        posts#update
                  DELETE /posts/:id(.:format)                        posts#destroy
```

The below graphic represents the implementation of the REST architecture in Rails and explains the URL, the controller action and purpose of each RESTful route.

![Rails RESTful Routes Table](/images/routes_rest.png "Rails RESTful Routes Table.")


The root directory or home page will appear on localhost:3000.  To set the root directory to a view other than the default root directory, use  root :to => 'Controller#action'.  You can set the root directory as a separtate landing page but for our application we will set the posts index page as the root directory by entering root :to => "posts#index" in  routes.rb. 

```ruby Routes.rb
Rails.application.routes.draw do

  root :to => "posts#index"

  resources :posts do
    resources :comments
  end

end
```
In order to see anything on localhost:3000 we will now need to create our post#index view.  We do this by creating and ```index.html.erb``` file in the app/views/posts folder. In this view we will render all of our posts. We will use embedded ruby.  For now lets just write the following in the ```index.html.erb``` file

```
<h1>All Blog Posts</h1>
```
if you go to localhost:3000 you should now see All Blog Posts displayed on your root page.

##CRUD Post and Comment Controllers
We now need to completely set up our controllers so that we can add, edit, show and delete post and be able to create a comment. We will take care of these actions in our controller which will allow us to collect information and display the information in the view.  I will show what should go into each section and explain what it does and what view it will render.

Your Posts Controller should look like the following

```ruby posts_controller.rb
class PostsController < ApplicationController
  before_action :set_post, only: [:show, :edit, :update, :destroy]
 
  # GET/posts
  # posts/index.html.erb which displays all posts
  def index
    @posts = Post.all
  end

  # GET /posts/new
  # posts/new.html.erb which shows and HTML form to create a new post
  def new
    @post = Post.new
  end

  # POST /posts
  # posts/index.html.erb  
  # This saves information to the database after after new post is created from new.html.erb
  # redirects to index page where new post will appear
  def create
    @post = Post.new(post_params)
 
    if @post.save
      redirect_to @post, notice: 'Post was successfully created.'
    else
      render :new 
    end
  end
 
  # GET /posts/1   
  #posts/show.html.erb
  # this will show an individual post for an newly created or updated post
  # will also contain the comment form to create a new comment and show comments
  def show
    @comment = Comment.new
  end
 
  # GET /posts/1/edit   
  # edit.html.erb
  # This is an HTML form for editing a post
  def edit
  end

 
  # PATCH/PUT /posts/1
  # posts/show.html.erb
  # save the information from edit.html.erb to the database and updates a specific post
  # it will then render the show.html.erb page
  def update
    respond_to do |format|
      if @post.update(post_params)
        format.html { redirect_to @post, notice: 'Post was successfully updated.' }
        format.json { render :show, status: :ok, location: @post }
      else
        format.html { render :edit }
        format.json { render json: @post.errors, status: :unprocessable_entity }
      end
    end
  end
 
  # DELETE /posts/1
  # posts/show.html.erb
  # The deletes a specific post and removes it from the database
  def destroy
    @post.destroy
    respond_to do |format|
      format.html { redirect_to posts_url, notice: 'Post was successfully destroyed.' }
      format.json { head :no_content }
    end
  end
 
  private
    # Use callbacks to share common setup or constraints between actions.
    def set_post
      @post = Post.find(params[:id])
    end
 
    # Strong parameters, to only allow specific attributes to be updated and saved to database.
    def post_params
      params.require(:post).permit(:title, :content)
    end
 
end
```

Your Comments Controllers you should look like the following.  This will allow you to create a new comment and save it to the database and redirect you the comment for that post.

```ruby comments_controller.rb
def create
    @post = Post.find(params[:post_id])
    @comment = @post.comments.build(comment_params)
 
    if @comment.save
      redirect_to @comment.post
    else
      redirect_to :back
    end
  end
 
  private
    def comment_params
      params.require(:comment).permit(:commenter, :content)
    end
```

## Views 
Now that the controllers and routes have been set up for the Posts and Comments the only thing
left to do now is to set up the view.  As stated before we will use the embedded ruby to create 
the templates for all of the views that correspond to the controller actions.  You can see all of the views in the [GitHub Repository](https://github.com/denineguy/blog).















