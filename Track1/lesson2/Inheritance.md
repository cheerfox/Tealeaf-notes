#Inheritance
Here we're extracting the `speak` method to a superclass `Animal`, and use inheritanece to amke that behavior available to `GoodDog` and `Cat` classes.

```ruby
class Animal
  def speak
    "Hello!"
  end
end

class GoodDog < Animal
end

class Cat < Animal
end

spark = GoodDog.new
paws = Cat.new
puts sparky.speak    # => Hello!
puts paws.speak      # => Hello!

```
We use `<` operaor to signify inheritance. All method in `Animal` are available to the class `GoodDog` and `Cat`.  
But we can also **override** the method inheritanced by the superclass, Because Ruby find the method by check the **method lookup path**.

##Reserved word `super`
When you call `super` from within a method, it will search the inheritance heirachy for a method by the same name and then invoke it.

```ruby
class Animal
  def speak
    "Hello!!"
  end
end

class GoodDog < Animal
  def speak
    super + "from GoodDog class."
  end
  
sparky = GoodDog.new
sparky.speak   # => "Hello! from the GoodDog class"
```

Anothor more common way of using `super` is with `initalize`.

```ruby
class Animal
  attr_accessor :name
  
  def initialize(name)
    @name = name
  end
end

class GoodDog < Animal
  def initialize(name, color)
    super(name)
    @color = color
  end
end

bruno = GoodDog.new('Bruno', 'Brown')   
```

##Mixing in module
```ruby
module Swimmable
  def swim
    "I'm swimming"
  end
end

class Animal; end

class Fish < Animal
  include Swimmable
end

class Mammal < Animal
end

class Cat < Animal
end

class Dos < Animal
  include Swimmable
end
```
Now `Fish` and `Dog` objects can swim~

**A common naming convention for Ruby is use "able" suffix on verb describe the behavior that the module is modling**

##Inheritance
**subclass** and **module** are the two way to implements Ruby.Here are a couple of things to remember when evaluating those two choices.  

- You can only subclass from one class. But you can mix in as many module as you'd like.  
- If it's an "is-a" relationship, choose class inheritance. If it's a "has-a" relationship use module. Ex: a dos is an animal, has an ability to swim.
- You can not initiate modules.

###More method lookup path

```ruby
class Animal
  include Walkable

  def speak
    "I'm an animal, and I speak!"
  end
end
class GoodDog < Animal
  include Swimmable
  include CLimbalbe
end

puts "GoodDog method lookup path"
puts GoodDog.ancestors
```
This will out put: 

	GoodDog
	Climbable
	Swimmable
	Animal
	Walkable
	Object
	Kernel
	BasicObject
Above tell us: 

- Ruby look at the **last** module we include **first**
- All `GoodDog` objects will have access to not only `Animal` methods, but also methods defined in the `Walkable` module, as well as all other modules mixed into any of it's superclasses.
