# Adapter

### Overview
_The adapter is a structural design pattern that allows two objects with incompatible interfaces to work together._

### Real World Example
Before I explain what it is and how to use the adapter pattern, think about what this name associates you with. If you’ve ever been to a country that uses other electrical sockets, you must have used an adapter similar to the one in the picture below.

So what is the purpose of using an adapter? The adapter allows you to connect a plug(Adaptee) to an electrical socket(Target) that do not fit together.

### Coding Problem
First, we will create a simple **User** class that has two attributes, **first_name** and **last_name**, and a **full_name** method that returns the full name of the user.

``` ruby
class User
  attr_reader :first_name, :last_name

  def initialize(first_name, last_name)
    @first_name = first_name
    @last_name = last_name
  end
  
  def full_name
    "#{first_name} #{last_name}"
  end
end
```


Next, we will create a **Greeter** class, with a **hello** method that will allow us to greet the user.

``` ruby
class Greeter
  def self.hello(user)
    puts "Hello, #{user.full_name}!"
  end
end

mike = User.new('Michael', 'Scott')
Greeter.hello(mike) # => Hello, Michael Scott!
```

As we can see, everything in the example above works fine. Now let’s assume that we also want to say hello to every team. So let’s create a **Team** class.

``` ruby
class Team
  attr_reader :name

  def initialize(name)
    @name = name
  end
end
  
development = Team.new('Development')
Greeter.hello(development) # NoMethodError: undefined method `full_name'
```


The method will of course raise an error because the **Team** class doesn’t have a method that returns the full name.

### Solution
Before we create a solution to this problem, we need to define three objects that we need in the **Adapter** pattern.

#### Target
The object we want to use. In this case, it will be the **Greeter** class.

#### Adaptee
The object we want to adapt to work with the target. In this case, it will be the **Team** class.

#### Adapter
An object that allows **Adaptee** to connect to a **Target**.

If we have already defined **Target** and **Adaptee** objects, we can prepare our **Adapter** class. To create it, just wrap **Adaptee(Team)** with a new class**(Team Adapter)** and then create a method**(full_name)** that will be compatible with the **Greeter** class interface.\

``` ruby
class TeamAdapter
  attr_reader :team
  
  def initialize(team)
    @team = team
  end
  
  def full_name
    "#{team.name} Team"
  end
end
```

An **adapter** written in such a way can easily be used to work with the **Greeter** class.

``` ruby
dev_adapter = TeamAdapter.new(development)
Greeter.hello(dev_adapter) # => "Hello, Development Team!"
```

### Pros:
- Communication interface is separated from business logic (SRP)
- You can add new adapters to existing code without breaking it (OCP)

### Cons:
- Increase complexity. Sometimes it’s better rebuild the class

### More reading
- https://refactoring.guru/pl/design-patterns/adapter
- https://medium.com/@kroolar/design-patterns-in-ruby-adapter-ed62abdf87d

