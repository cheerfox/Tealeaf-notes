##Docs
1. http://api.rubyonrails.org/classes/ActiveRecord/Associations/ClassMethods.html
2. http://guides.rubyonrails.org/routing.html
3. http://guides.rubyonrails.org/layouts_and_rendering.html

##Bundle
Bundle deal with the gem version varity among many rails projects.
**Bundle install gem into a sandbox individually for every each project**.
So the same gems in different project will be **independent**.

If you use `rake db:migrate` you are using the **system** rake not project specific, so if you got a error when using `rake db:migrate`, maybe it's because you system rake version is not fit to the project version, so try to use `bundle exec rake db:migrate`.

##database.yml
**This file is very important**
the same as **Gemfile** and **routes.rb**
It's tells us how the rails project connect with the **Database** and which database you use in the project.

##sqlite3
We use sqlite3 database in rails deafault, **the database of sqlite3 is a file** `db/development.sqlite3` in our code base, **not like** most fo relational database ex: mysql, they are usually on an database server and maybe you'll need usename or password to access the database.

##little things
 - All controller subclass the Application controller
 - The file in `config/intializer` will execute before everything else so, maybe some third-party library.
 - `.`use in the beginning of a file name, it's means the file is hidden
 - Don't use generator
 - The default in controller action is **render template** implicitly, if you want to **redirect**, you should do it explicitly,so ypu can also code
 - In 1-M relation foreign key is always in the **M side** and can repeat the same data.
 - Use `bundle install --without production` just to install the gems in development need, no gem use in production be install
 - Use `rvm gemset` individual from each projects
 - Every time you can the schema or database try that in rails console to check if it is woking correctly
 - id to id , object to object
 - `rake db:migrate` just go to see in the database and see the migration version record and find out which migration file hasn't been migrate, then update the record in database
 - use `reload!` in the rails console after you change your model related files
 
```ruby
def index
	@post = Post.all
	render 'posts\index.html.erb'  # This line is implicitly done by rails so you don't need to code it
end
```
##Naming convention
 - **plural and lowercase** for **Table name**
 - **singular and lowercase** for **model file name**

##Rails request response diagram
![](http://d3ncao0pifc37i.cloudfront.net/images/request_response_mvc.jpg)


#Add migration
1. `rails g migration create_users` gernerate a migration file for create a users table (DB layer), the migration name can be anything, but if you use create_tablename, then rails will recognize it, and automatically add create_table function in your migration file
2. add `t.string :username` and `t.timestamps`in the migration file, and execute `rake db_migrate` then the table has been create (DB layer)
3. Then the `model layer`, create a file called `user.rb` (model layer)
4. `timestamps` in the migration file automatically add the `create_at` and `update_at` these two attribute to the table, remeber to user `timestamps`
5. https://ihower.tw/rails4/migrations.html
6. `change` method in the migration file in create_table is equal to `up` + `down` method
7. Make sure your migration run cleanly and not impact to each other end to end
8. `migration` is **Modify the schema of a database in a sysytematic way**
9. **Don't go to change the past migration** it will cause some conflict
10. In rails there is a table record which migration you have already migrated, so don't use migration roll back, you will get conflict when you work with your co-worker
11. So don't modify the migration file, just add migtation
12. Use migration to insure the correct schema changes process between several mechines
13. You alao can chage the schema without migration, but **Don't do that ~~**

```ruby
##user.rb
class User < ActiveRecord::Base

end
```

## 1-M relation in ActiveRecord layer

```ruby
class User < ActiveRecord::Base
	has_many :posts      #You don't need to specify the foreign key default is user_id
							#But if the foreign key is not the deault you need to specify it
end

class Post < ActiveRecord::Base
	belongs_to :user	    #You don't need to specify the foreign key default is user_id

end

##If you want to break the convention and use another name try this below, that is we can change the default name of virtual attribute

class Post < ActiveRecord::Base
	belongs_to :creator, foreign_key: 'user_id', class_name: 'User'
end

#Then you can use post.creator to pull out the user who create the post

```
After these association done you will have the **virtual attribute** and the **getter and setter method on it**
ex: You can do this `user.posts` and it will get the array like data structure contains the collection of posts belongs to a particular user, or `post.user` to get the user of a particular post, **these are virtually attribute** which is not in the table, and you can get or set it after you specify the relation in the **ActiveRecord layer**
You can even do this `post.user.posts`
![](http://d3ncao0pifc37i.cloudfront.net/images/1-M_association.png)


##ActiveRecord pattern
![](http://d3ncao0pifc37i.cloudfront.net/images/ar_db_connection.jpg)

##M-M relation
###habtm vs hmt(Always use hmt)
1. has_and_belongs_to_many
	- no join model
	- implicit joim table at db layer call table1_name_table2_name
	- Problems : can't put additional attribute on join table
2. has_many :through
	- has a join model
	- can put additional attribute on join table

- Use `'PostCategory'.tableize` to figure out the table name default in rails

![](http://d3ncao0pifc37i.cloudfront.net/images/M-M_association.png)

1. First create a join table with both primary keys in the join table
2. set association

```ruby
#post.rb
class Post < ActiveRecord::Base
	has_many :post_catogories    
	has_many :categories,   through: :post_categories   #This line is dependent to the last line so it can't appear before the last line
end

#post_category.rb
class PostCategory < ActiveReord::Base
	belongs_to :post
	belongs_to :category
end

#category.rb
class Category < ActiveRecord::Base
	has_many :post_categories
	has_many :posts, through: :post_categories
end
```
##Routes and Resource

```ruby
#In routes.rb
PostitTemplate::Application.routes.draw do

	get '/posts', to: 'posts#index'
	get '/posts/:id', to: 'posts#show'  #':id' you can also choose antoher name like ':whatever', and you can access it use 'params[:whatever]', this is the informations from the url
	get '/posts/new', to: 'posts#new'
	post '/posts', to: 'post#create'
	get '/posts/:id/edit', to: 'posts#edit'
	patch '/posts/:id', to: 'posts#update'
	
	##You can replace above by use this below
	resources :posts, except: [:destroy]

end

```
Do not to expose the unecessary url, just use `excpet: [:action]` or `only: [:action]`

##Partial
The file name of the partial muse be strated with `_` like `_form.html.erb`

```ruby
	<%= render 'shared/content_title', title: '@post.title'   #=> This means you have a partial called _content_title.html.erb in your shared folder in views folder, and has a local variable called title.

```

If you have an array of object need to pass in the partial then

```ruby
<%= render @posts =%>  #=> look for the partoal in views/posts/_post.html.erb, them it will pass the element of array one by one to the partial
```
The code in Line1 is equal to belows code

```ruby
<% @post.each do |post| %>
	<%= render 'posts/post', post: post =%>
<% end %>
```


##Work flow
1. build the view individually
2. Extract the common pattern to the partials like below

```ruby


```






