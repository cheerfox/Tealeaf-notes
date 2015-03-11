#OOP
##Encapsulation
Hiding pieces of functionality and make it unavailable to the rest of code base.  It is a form of data protection, but we can use the interface(method) to interact with them(object)

##Polymorphism
Is the ability for data to be represented as many different types.
###Inheritance
Define the basic classes with large **reusablility** and smaller **subclass** for more fine-grained, detailed behaviors.
###Module
1. similiar to class
2. You cannot create an object with a module
3. A module must be mixed in eith a class using the reserved word ```include```
4. This is called a **mixin**, after mixing in a module, the behaviors declared in that module are available to the class and its objects.

##Everthing in Ruby is Object
Use ``.class`` we can find out the which class the object belongs.
	
	irb :001 > "hello".class  
	=> string
###Classes define Objects
Use **CamelCase** to define a class  

```ruby
class GoodDog   #CamelCase
end

sparky = GoodDog.new
```
Above, We new an object named ``sparky`` from the class ``GoodDog``  

![](http://d2aw5xe2jldque.cloudfront.net/books/ruby/images/class_instance_diagram.jpg)

###Modules
A module is a collection of behaviors that is useable in other class via mixin

```ruby
module Speak
  def speak(sound)
    puts "#{sound}"
  end
end

class GoodDog
  include Speak
end

class HumanBeing
  include Speak
end

sparky = GoodDog.new
sparky.speak("Arf!")     # => Arf!
bob = HumanBeing.new
bob.speak("Hello!")     # => Hello!
```

###Method Lookup
We can use the ``ancestors`` method on any **class** to find out the method lookup chain.

``GoodDog.ancestors``  

The output looks like this:

	---GoodDog ancestors---
	GoodDog
	Speak
	Object
	Kernel
	BasdicObject




