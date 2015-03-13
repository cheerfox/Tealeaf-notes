#Classes and Objects
##States and Behaviors
When defining a class, we focus in two things: states and behaviors. **States** track attributes for individual objects. **Behaviors** are what objects are capable of doing(method).

##Initializing a New Object
**Initialize** method

```ruby
class GoodDog
  def initialize
    puts "This object was initialized!"
  end
end

sparky = GoodDog.new     # => "Thos object was initialized!!"
```

The ``initialize`` (like constructor)method gets called every time you create a new object.

##Instance Variable

```ruby
class GoodDog
  def initialize(name)
    @name = name
  end
end
```

``@name`` is called instance variable. It's a variable that exist as long as the object instance exist ans it's one of the way we tie data to objrcts.
Use: 

```ruby
sparky = GoodDog.new("Sparky")
```

Here, the string "Sparky" is being passed from the `new` method through to the `initialize` method, and its assigned to the local vaiable `name`

If we want to access instance varaible like `puts sparky.name`  
We will got **Error**:

	NoMethodError: undefined method `name' for #<GoodDog:0x007f91821239d0 @name="Sparky">

So we need a `get_name` method, we can only use the **Method** to access the Intance Variable. also we need a `set_name` method to change the name of `@name`

```ruby
class GoodDog
  def initialize(name)
    @name = name
  end
  
  def get_name
    @name
  end
  
  def set_name=(name)
    @name = name
  end
  
  def speak
    "#{@name} says arf!!"
  end
  
end

sparky = GoodDog.new("Sparky")
puts sparky.speak
puts sparky.get_name
sparky.set_name = "Spartacus"
puts Sparky.get_name
```

You can treat `set_name=` as a method name, Ruby can recognize it as a setter.

Finally,  as a convention, Rubyisists want to name those getter and setter methods using the same name as the instance variable.

```ruby
class GoodDog
  def initialize(name)
    @name = name
  end
  
  def name
    @name
  end
  
  def name=(name)
    @name = name
  end
  
  def speak
    "#{name} says arf!!"
  end
  
end

sparky = GoodDog.new("Sparky")
puts sparky.speak
puts sparky.name            # => "Sparky"
sparky.name = "Spartacus"
puts sparky.name            # => "Spartacus"
```

###Built in way to set getter and setter for instanve variable

```ruby
class GoodDog
  attr_accessor :name
  
  def intialize(name)
    @name = name
  end
  
  #....
```

The `attr_accessor`method takes a symbol as an argument, which it use to create the method for `getter `and `setter` method

####All the `attr_*`method take a `Symbol` as parameters
	attr_accessor :name, :height, :weight
	
###For setter
```ruby
def change_info(n ,h, w)
  name = n  #try to use setter method but not work!!!
  height = h
  weight = w
end

```
Ruby will treat it as an local varialbe declare, not what we want to do
so to need to add `self` keyword

```ruby
def change_info(n, h, w)
  self.name = n
  self.height = h
  self.weight w
end
```
We can also add `self` before the getter but it's not **necessary**.


###Final Version of Class

```ruby
class GoodDog
  attr_accessor :name, :height, :weight

  def initialize(n, h, w)
    @name = n
    @height = h
    @weight = w
  end

  def speak
    "#{name} says arf!"
  end

  def change_info(n, h, w)
    self.name = n
    self.height = h
    self.weight = w
  end

  def info
    "#{name} weighs #{weight} and is #{height} tall."
  end
end

sparky = GoodDog.new('Sparky', '12 inches', '10 lbs')
puts sparky.info      # => Sparky weighs 10 lbs and is 12 inches tall.

sparky.change_info('Spartacus', '24 inches', '45 lbs')
puts sparky.info      # => Spartacus weighs 45 lbs and is 24 inches tall.
```
