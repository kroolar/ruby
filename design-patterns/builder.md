# Builder

_Creational design pattern that allows create complex objects step by step._

### Introduction
When you hear the word **builder**, you probably think of a construction worker however, in this case, we should think of a builder as an object that is able to build new objects based on multiple elements. Imagine a restaurant that sells **Tortillas**. The owner had the idea that each customer would be able to compose their own tortillas and gave the customers several options to choose from:

- **Meat**: chicken or mutton
- **Vegetables**: cucumbers, tomatoes, and onions
- **Sauce**: mild or spicy
- **Spicy** sauce has 3 levels: medium, hot, and extra hot

### Code Example
So let’s create a **Tortilla** class that will have attributes such as **meat**, **sauce**, and **vegetables** that will be an array.

``` ruby
class Tortilla
  attr_accessor :meat, :vegetables, :sauce
  
  def initialize
    @vegetables = []
  end
end
```

In addition, we will create a few empty classes that will be represented by individual ingredients.

``` ruby
class Chicken; end
class Mutton; end
class Cucumber; end
class Onion; end
class Tomato; end
class MildSauce; end
```

We will also add an attribute to a **spicy sauce** that will determine its level of spiciness.

``` ruby
class SpicySauce
  attr_reader :level

  def initialize(level)
    @level = level
  end
end
```

Now we need to prepare a **builder** class that will help us create our tortilla.

``` ruby
class TortillaBuilder
  attr_reader :tortilla

  def initialize(tortilla)
    @tortilla = tortilla
  end
 
  def select_chicken
    tortilla.meat = Chicken.new
  end
  
  def select_mutton
    tortilla.meat = Mutton.new
  end
  
  def add_cucumber
    tortilla.vegetables << Cucumber.new
  end
  
  def add_tomato
    tortilla.vegetables << Tomato.new
  end
  
  def add_onion
    tortilla.vegetables << Onion.new
  end
  
  def select_garlic_sauce
    tortilla.sauce = GarlicSauce.new
  end
  
  def select_spicy_sauce(level = :hot)
    tortilla.sauce = SpicySauce.new(level)
  end
end
```

Now, using the **builder** class, we want to create a **tortilla** that will contain:

- _Chicken meat_
- _Two cucumbers_
- _One Tomato_
- _Extra spicy sauce_

``` ruby
builder = TortillaBuilder.new(Tortilla.new)

builder.select_chicken
builder.add_cucumber
builder.add_cucumber
builder.add_tomato
builder.select_spicy_sauce(level: :extra_hot)
```

At this point, we have created an interface that allows us to quickly and easily create a new object with freely chosen properties.

### More reading
- https://refactoring.guru/design-patterns/builder
- https://medium.com/@kroolar/design-patterns-in-ruby-builder-5e65c451259f
