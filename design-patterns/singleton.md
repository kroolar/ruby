# Singleton

_Creational pattern responsible for the fact that a class can only have one instance._

### Real World Example
As an example of today’s pattern, I chose the head of the Catholic Church, the **Pope**. However, I do not mean the **Pope** as a person, but as an institution. As we know from history the first **Pope** was St. Peter, and from that time to the present day, this office is held by various persons. However, it is impossible for more than one **Pope** to exist at the same time. This institution imposes in advance that this position may only be occupied by one person at a time.

### Code Example
Let’s move the above example to the programming world and create a **Pope** class that can pray through the **pray!** method.

``` ruby
class Pope
  def pray!
    puts 'In nomine Patris, et Filii, et Spiritus Sancti...'
  end
end

pope = Pope.new
pope.pray! # => 'In nomine Patris, et Filii, et Spiritus Sancti...'

new_pope = Pope.new
new_pope.pray! # => 'In nomine Patris, et Filii, et Spiritus Sancti...'

pope == new_pope # => false
```

As we can see with the standard approach, we are able to create more than one instance of the Pope class. This is why we need to use the Singleton pattern to prevent creating more than one instance of a given class.

``` ruby
class Pope
  @@instance = new
  
  def self.instance
    @@instance
  end

  def pray!
    puts 'In nomine Patris, et Filii, et Spiritus Sancti...'
  end
  
  private_class_method :new
end

pope = Pope.new # => NoMethodError: private method 'new' called for Pope:Class
Pope.instance.pray! # => "In nomine Patris..."
```

As you can see in the example above, we can’t directly create a class with the **new** method, which is now **private**, and we need to use the **instance** method instead. By comparing two instances to each other, we can make sure that they are the same instance.

``` ruby
pope = Pope.instance
new_pope = Pope.instance
pope == new_pope # => true
```

**Singleton** pattern is so popular that there is a **Singleton module** in the **standard Ruby library**, thanks to which we will not have to focus on pattern configuration.

``` ruby
class Pope
  include Singleton
  
  def pray!
    puts 'In nomine Patris, et Filii, et Spiritus Sancti...'
  end
end
```

This code works in exactly the same way as the code presented at the beginning of this article.

### More reading
- https://refactoring.guru/design-patterns/singleton
- https://medium.com/@kroolar/design-patterns-in-ruby-singleton-b08db24669fb
