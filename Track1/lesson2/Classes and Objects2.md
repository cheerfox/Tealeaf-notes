#Classes and Objects-PartII

##Class Methods
Class methods are method we can call directly on the class itself.  
When defining a class method, we prepend the method name with the reserved word `self.`,like this:

```ruby
#example for class method
def self.what_am_i
  "I'm a GoodDog class!!"
end
```
Then we can call

```ruby
GoodDog.what_am_i    # => I'm a GoodDog class!
``` 
Objects caontain state, and if we have a method that does not need to deal with states, then we can just use a class method.

##Class varaiable
A varaible relate for an entire class we ues `class variable`

```ruby
class GoodDog
  @@number_of_dogs = 0
  
  def initialize
    @@number_of_dogs += 1
  end
  
  def self.total_number_of_dogs
    @@number_of_dogs
  end
end
  puts GoodDog.total_number_of_dogs    # => 0
dog1 = GoodDog.new
dog2 = GoodDog.new

puts GoodDog.total_number_dogs  # => 2

```
This is an example of using a class variable and class method to keep track of a class level detail that pertains only to the class, and not for individual objects.

##Constant

```ruby
class GoodDog
  DOG_YEARS = 7
  attr_accessor :name, :age
  
  def initialize(n, a)
    self.name = n
    self.age = a * DOG_YEARS
  end
end
```
Also we use setter method in our constructor

##The to_s method
When we use `puts` or `#{}` string interpolation, ruby will automatically call the method to_s on the object. In default, `to_s` method prints the object's class and an encoding of the object id.  

	puts sparky    # => #<GoodDog:0x007fe542323320>
	
But we can override it.

```ruby
class GoodDog
  def to_s
    "This is...."
  end
end
```
Then,

	puts sparky   #  =>  This is.....

Aonther method `p` , Ruby call `inspect` on it argument.

	p sparky   # =>  #<GoodDog:0x007fe54229b358 @name="Sparky", @age=28>
	
`p` is very good use in debug

##More about self
Within a class

1. `self`, **insied fo an instance method**, references the object that called the method. Therefore, `self.weight=` is the smae as `sparky.weight=`, in our example.
2. `self`, **outside of an instance method**, references the class and can be used to defie class methods. Therefore, `def self.class_method` is the same as `def GoodDog.class_method`

#Summary
- Initializing classes with the new method
- How instance variablrs keep track of an object's state
- Learning how `attr_*` methods generate **getter** and **setter**
- Instance method for object level
- class method for class level
- Constant in class
- How the`to_s` method is used by `puts``#{}`
- self