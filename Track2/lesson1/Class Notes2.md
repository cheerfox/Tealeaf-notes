##Mass assignment
Ex:
`Post.create({title: "yahoo", url: "yahoo.com.tw"})`
and you can leave without the `{}`
like `Post.create(title: "yahoo", url: "yahoo.com.tw")`

##Little thing
1. You can't use pure html gorm to post the data to the Rails applicaiotn because of this in the `applicaiotn_controller` is `protect_from_forgery with: :exception` this identify a sepcific token gerenate by rails form so, youcan not just use pure html form to post
2. You can use `binding.pry` in the beginning of the action for the controller, then you can see what is in the `params` hash
3. If you use **Model-backed form** you only can use the attribute name of that object to be the name of input, because it's for **mass assignment**, otherwise you will get error

##Rails form(Three way)
###Pure HTML

```html
<form action='/posts' method='POST'>
	Title<input type='text' name='title'>
	<input type='submit'>
</form>

```
This doesn't work because this in the `applicaiotn_controller` is `protect_from_forgery`, never so this in rails

###Rails Form helpers

```ruby
<%= form_tag '/posts' do %>
	<%= labal_tag :title %>
	<%= text_field_tag :title %>
	<%= submit_tag "Create Post" %>
<% end %>

```

###Model backed helper

```ruby
<%= form_for OBJECT do |f| %>
  <%= f.label :title %>
  <%= f.text_field :title %>
  <br>
  <%= f.submit class: 'btn btn-primary'%>
<% end %>

#you need to use strong parameter in controller!

```

###Model back form for nest resource
```ruby
  <%= form_for [@post, @comment] do |f| %>  ##This will post to /posts/:post_id/comments
    <div class="controll-group">
      <h5>New Comment</h5>
      <%= f.text_area :body, rows: 5 %>
    </div>
    <%= f.submit class: 'btn btn-primary' %>
  <% end %> 

```


##strong parameter

```ruby
def post_params
	params.require(:post).permit!
end

#This will return a post hash
#use string paramter or use attr_accessible :title in model layer to let :title can be mass assignment
```

##validates

```ruby
	#In post.rb
	validates :title, presence: true
```
In rails console you can do this
`post = Post.create(url:'www.google.com')`
Then you will get an error then you can use
`post.errors.full_messages` to retrieve the error messages on the object itself, errors.full_messages will return a array for error messages

##Create action and validates

```ruby
#In PostsController
def create
	@post = Post.new(post_params)
	if @post.save
		redirect_to posts_path
	else   #if validates error
		render 'new'
	end
end

```

```ruby
#In post.rb

validates :title, presence: true

```
Above why do we use **render** except using **redirect_to** because **error messages is on the @post instance varaible so if we use redirect_to it will create a new request and the instance error message will disappear**

##form_for method
`<%= form_for @post do %>`

If @post is a existsting object then form_for methos will map to the `/posts/:id` routes, and create a hidden input elemet with a name `_method`, and value with **patch**

If @post is a new object then map to `/posts` routes.
form_for method will map to the right route by looking at the object, so `new` and `edit` view can be identical, so we can use `partial on it`.



##Little things
1. redirect_to => URLs
2. render => template file
3. if you want to pass html element into a string remember to use `html_safe` method
4. If you want to render a template in one action of a controller you must insure that all `instance variable` use in that template must be set in the action before rendering template

##More M-M association
###Use the setter method like category_ids
ex: `post.category_ids = [] `, this delete the accociation of categories in that post, or `post.category_ids = [1,2,3]` reset the association of category in that post
###Strong parameter with array input
See the example about category_ids

`params.require(:post).permit(:title, :url, :description, category_ids: [])`

###Use collection_check_boxes or collection_select to generate the html checlbox for collection
It will generate

```html
<label class="checkbox inline" for="post_category_ids_2">
	<input checked="checked" class="checkbox" id="post_category_ids_2" name="post[category_ids][]" type="checkbox" value="2">Web
</label>
```
Then if you submit the category_ids will be in the post hash in params hash



##Helper
extract the comment logic to helper

##non backed model form VS backed model form
Model backed forms are useful when performing "CRUD" actions on a resource, and non-model backed form helpers are used everywhere else. 

####todo
1. create action of comment
2. new comment form in post show
3. error partial on comment form
4. add fix_url to application helper method  
5. play with check box helper