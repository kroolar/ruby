# Prototype

_A creational design pattern that allows you to create a copy of the object._

### Real World Example
Sometimes the name Clone can be found instead of the name Protype, and this is what I will use for the example below because, in my opinion, it is more appropriate. So we probably all know the story of the first cloned sheep named Dolly(https://en.wikipedia.org/wiki/Dolly_(sheep)). But what does it mean she was a clone? Which means she was biologically the same as the sheep from which the cells were taken (let’s call her Molly).

So, to create our clone(**Dolly**), we need two things:

The object we can clone(**Molly**)
A method that will allow us to clone this object(**clone**)

### Coding Problem
First, let’s define a Sheep class that will allow us to create a new sheep.

``` ruby
class Sheep
  attr_reader :genome, :name
  
  def initialize(name:)
    @name = name
    @genome = Random.new_seed
  end
end

molly = Sheep.new(name: 'Molly')
molly.genome # => 58277341567052563563790228364424051023

dolly = Sheep.new(name: 'Dolly')
molly.genome == dolly.genome # => false
```

As you can see in the example above, I used a special method that assigns a unique genome encrypted in the form of a sequence of 39 digits. At this point, we are unable to create objects with two identical genomes.

### Solution
Using the **Prototype** pattern, we will add a **clone** method to our class that will allow us to clone our object. This method will be responsible for assigning the same genotype to the newly cloned sheep. To create a new clone, we will also have to enter a new name for our new sheep.

``` ruby
dolly = molly.clone(name: 'Dolly')
molly.genome == dolly.genome # => true
```

This method allows you to easily clone our sheep and give it a new name.

### Caution
However, it should be remembered that in **Ruby** the **Prototype** pattern is implemented in the standard library, so when creating the clone method, we overwrite the existing one. In some cases, overriding this method can be useful because the default method only makes a shallow copy of the object.

### Pros:
- You can clone objects without coupling to their concrete classes
- You can get rid of repeated initialization code in favor of cloning pre-built prototypes
- You can produce complex objects more conveniently
- You get an alternative to inheritance when dealing with configuration presets for complex objects

### Cons:
- Cloning complex objects that have circular references might be very tricky

### More reading
- https://refactoring.guru/design-patterns/prototype
- https://www.rubyguides.com/2018/11/dup-vs-clone/
- https://apidock.com/ruby/v2_6_3/Object/clone
- https://medium.com/@kroolar/design-patterns-in-ruby-prototype-ad034ad0e9fb
