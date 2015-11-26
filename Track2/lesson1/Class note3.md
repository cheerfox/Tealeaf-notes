##assets pipeline
Rails use `Sprocket` to manage asset pipeline dedault
It's will see the manifest file and include all css and js file we need and compress, compile to the **/public/assets** with one css file and one js file,to do this use `rake assets:precompile`

##Authentication
User Rails built in `has_secure_password`

1. add a column `password_digest` in `users` table
2. add a line `has_secure_password`
3. then you will have a setter method `passowrd` for any User object and a `authenticate('string')` method for verigy the password
4. Rails will use bcryt-ruby to hash the password to a long string and save it to the database

##validate only when we create

```ruby
#In user.rb
class User < ActiveRecord::Base
	validates :password, presence: true, on: :create, length: {minimum: 3}
end

```
Bay the way we can also validates on virtual attribute like `password` in users

##memoization!!!!!!!

```ruby
  def current_user
    @current_user ||= User.find(session[:user_id]) if session[:user_id]
  end
```
It's an optimization, if the instance variable exsist we won't execute the code after that.
If you have a function will repeatly many times in one request, you can do this optimzation

##where vs find_by

```ruby
#This will return an array even if it just on result
user = User.where(username: params[:username]

#This will reutrn the first item in result array
user = User.find_by(username: params[:username])
```

##Method `helper_method` in application controller

```ruby
class ApplicaitonController

	helper_method :current_user

	def	 current_user
	end
end
```
This make `cuurent_user` not only availible in Controllers but also in the view templates

##polymorphic association!!!!!!
anther way to implement **1-M** associaiton. ex: comment can association with post or photo or video, we can have multiple choice on one side but we only can choose one of them at a time

```ruby
#vote.rb   many-side
class Vote < ActiveRecord::Base
	belongs_to :voteable, polymorphoic: true
end
```

```ruby
class Post < ActiveRecord::Base
	has_many :votes, as: :voteable
end

```

This says Rails will look for two column for polymorphic `vateable_type`, and `voteable_id`
Then you can do `Vote.first.voteable = Comment.first` then the voteable_type will be "Comment" and voteable_id wiil be set as well

##special link_to syntax

```ruby
	#if you have some html element in your link name user this syntax
	<%= link_to user_path(@user) do %>
	<i class=icon-pencil></i>
	User Profile
	<% end %>
```

##member and colletion in routes
```ruby
resource :posts do
	member 
		post :vote
	end
	
	
	##cf to collection
	collection do
		get :archives
	end
	#This will generate route
	/posts/archives to posts#archives
end
```
member will generate the route post `/posts/:id/vote` to posts#vote
Member will generate a route which is relavent to each member of post in this case

##Turn a link to from get to post method!!!
```ruby
<%= link_to vote_post_path(post), method: 'post' do %>
	....
<% end %>
```
This will generate a a tag and has `data-method="post"` in it and there is another javascript in rails will change it to a form and post verb when you click it

##Set additional parameter in url

`user_path(@user, tab: 'comment', someting: 'else')`
This will generate url like `/users/1?tab=comment&something=else`

##If the method is according to buessniess logic not only the presentation layer please put the moethod in model layer

##redirect to back
`redirect_to :back`
redirect to the url you came from

##correct parameter to the url helper
In `vote_post_comment_path()`, it will generate the url like this `/posts/:post_id/comments/:id/vote`, throughing the url we know that we need to pass a post objet and a comment object to `vote_post_comment_path()` so that it will know the post_id and comment_id then it can generate the correct url

##validation for vote
`validates_uniqueness_of :creator, scpoe: [:voteable_id, :voteavle_type]`

##AJAX in Rails

```ruby
#In controller
def  vote
	respond_to do |format|
		format.html { redirect_to :vakc, notice: "...."}
		formst.js { #if it's a ajax call then will do this ,the deafult will render a js template called vote.js.erb, and in the js template you can also get to access the instance varaible you declare in thhis action}
	end
end

```

```ruby
#In the view
<%= link_to vote_post_path(post), method: 'post', remote: true do %>
	....
<% end %>

##remote: true , this will add date-remote attribute to the <a> tag then the rails javascript will set the ajax call to this element
```

##ActiveRecord callback
```ruby
class Post < ActiveRecord::Base

  after_validation :generate_slug


  def generate_slug
    self.slug = self.title.gsub(' ','-').downcase
  end

  def to_param
    self.slug
  end
end

```
We can execute method during the ActiveRecord live cycle

##URL Slug
1. make a slug column
2. decide when to generate the slug
3. make sure the exsisting data have their slug
4. override `to_param` method

```ruby
<%= link_to "#{post.comments.size} comments", post_path(post) %>
```

`post_path(post)` is equal to ` post_path(post.to_param)`


##Time Zone
##todo

##Extract the common module

First

```ruby
#In application.rb add

cobfig.autoload_paths += %W(#{config.root}/lib)

```
Then add a file **voteable.rb** in the lib folder

```ruby
In voteable.rb

module Voteable
	extend ActvieSupport::Concern
	
	included do 
		has_many :votes, as: :voteable
	end     ##given by Concern and it will execute when you include this module
	
	def total_votes
	
	end
	
	def down_votes
	
	end

end



```