#Note

1. Methods contain behaviors
2. Instance variables contain states
3. Object are **instantiated** from classes, and contain states and behaviors
4. The instance varaible of a class can also be a class
5. Don't use the same name as the instance varaible when you name the local varaible or it will get confused if it's a getter or local variable.
6. If you have some magic number which appear mutiple times in your program, try to make it as a **Constant varaible**

###Always use getter and setter instead using instance variable directly, In that way, we can have chance to do another type of getter and setter for some check in setter or some different display in getter

Ex: 

```ruby
class Dog
  attr_accessor :ssn, :height, :weight

  def update_info(n, h, w)
    self.ssn = n   #instead of using @ssn = n
    self.height = h
    self.weight = w
  end
  
  def ssn
    #555-55-555
    "xxx-xx-" + @ssn.split('-').last   #xxx-xx-555
  end
  
  def ssn=(new_ssn)
    if valid_format?(new_ssn)   ##we can add some format check in our setter
      @ssn = new_ssn
    end
  end
end

```  

- **Look the method loopup chain if you get the method doesn't function correctly, use `Class.ancestors`**
- Module only use to mixin to the classes or use it for namespacing
- You really don't make a module to be too large, it will become hard to include

##More module

```ruby
module Fetchable
  def fetch
    "#{name} is fetchig!"
  end
end
```
**In order to use this module, your class must respond to a `name` method call like:**

```ruby
class Dog
  attr_accessor :name
  #...
end
```

##Mixin
Use ofr multiple inheritance, just like inject common behaviors to the class, use `include` key words


##Tips for OOP BalckJack Game

 1. AHve detailed requirements ro specifications in written form.
 2. Extract major nouns -> classes
 3. Extract major verbs -> instance methods
 4. Group instance methods into classes
 
```ruby
class Card
  attr_accessor :suit, :value
  
  def initialize(s, v)
    @suit = s
    @value = v
  end
  
  def to_s
    "This is the card!  #{suit}, #{value}"
  end
end

class Deck
  attr_accessor :cards
  
  def initialize
    @card = []
    ['H', 'D', 'S', 'C'].each do |suit|
      ['2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K', 'A'].each do |face_value|
        @cards << Card.new(suit, face_value)
      end
    end
  end
  
  def scramble!
    cards.shuffle!    #cards -> getter method
  end
  
  def deal
    cards.pop
  end
end

class Player
end

class Dealer
end

class Blackjack   #like a game engine
  attr_accessor :player, :dealer, :deck
  
  def intialize
    @player = Player.new("Bob")
    @dealer = Dealer.new
    @deck = Deck.new 
  end
  
  def run
    deal_card
    show_flow
    #......
  end
end

Blackjack.new.run


```



