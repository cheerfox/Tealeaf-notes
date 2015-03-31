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
1. `rails g migration create_users` gernerate a migration file for create a users table (DB layer)
2. add `t.string :username` and `t.timestamps`in the migration file, and execute `rake db_migrate` then the table has been create (DB layer)
3. Then the `model layer`, create a file called `user.rb` (model layer)
4. `timestamps` in the migration file automatically add the `create_at` and `update_at` these two attribute to the table, remeber to user `timestamps`
5. https://ihower.tw/rails4/migrations.html
6. `change` method in the migration file in create_table is equal to `up` + `down` method
7. Make sure your migration run cleanly and not impact to each other end to end
8. `migration` is **Modify the schema of a database in a sysytematic way**
9. **Don't go to change the past migration** it will cause some conflict

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


#ActiveRecord pattern
![](http://d3ncao0pifc37i.cloudfront.net/images/ar_db_connection.jpg)

