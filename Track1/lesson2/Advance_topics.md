1. Everything in Ruby are `object` even if `class` itself
2. class `Dog` itself is also an object instaniated from class `Class`
2. `singleton class` is a class which shadow a object, so we can defin'e a method for a object, it will define at singleton class
3. **class method** will also be inheritance~~
4. By default method invokation will automatically prepend the `self` key word, so if `a_method` method invokation is the same as `self.a_method`

```ruby
class Vehicle
  def self.a_class_method           # self here is equal to Car
    puts "It's a class method from Vehicle"
  end
end


class Card < Vehicle

end

Card.a_class_method       # => "It's a class method from Vehicle"

#--------------------------------------------------
##singleton method

class Car
  attr_accessor :name
  def initialize(name)
    self.name = name       # => self here is equal to the object which all this instance object
  end
end

bob = Car.new('Bob')

def bob.a_singleton_method
  puts "A singleton method"
end
bob.a_singleton_method     # => A singleton method

```