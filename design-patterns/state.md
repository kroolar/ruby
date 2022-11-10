# State

“State is a behavioral design pattern that allows an object to change its behavior when its internal state changes.”

As the name suggests, this pattern helps us work with the states of a given object. For today’s example, let’s take the **states** that orders in a restaurant go through. Our order will go through **4 states**:

- New
- In Progress
- Ready
- Received

**New**
The first state appears when we create a new order. At this point, we will be able to perform two actions:
- pay for the order
- check order status

**In Progress**
This condition appears just after paying. In this state, we can only:
- check order status

**Ready**
A state that says our order is ready for pickup. At this point, we can take two actions:
- check order status
- take the order

**Received**
The last state appears after receiving the order. At this point, we can:
- check order status
- rate the order

### Code Example
We’ll first create a **State** class that implements all the methods we want to use on the **Order** class. Each of these methods will raise an error.

``` ruby
class State
  def initialize(order)
    @order = order
  end
  
  def pay
    raise 'You cannot use this method on this state'
  end

  def next_state
    raise 'You cannot use this method on this state'
  end
  
  def take
    raise 'You cannot use this method on this state'
  end
  
  def rate(number)
    raise 'You cannot use this method on this state'
  end
end
```

Having the main **State** class, we are able to create subclasses that will represent a particular **State** and implement only those methods that we want to use in a given state.

``` ruby
class NewState < State
  def pay
    puts 'Payment has been accepted'

    @order.state = InProgressState.new(@order)
  end
end

class InProgressState < State
  def next_state
    puts 'Order is ready to take'

    @order.state = ReadyState.new(@order)
  end
end

class ReadyState < State
  def take
    puts 'The order has been taken'

    @order.state = ReceivedState.new(@order)
  end
end

class ReceivedState < State
  def rate(number)
    puts "You rated the order to: #{number}"
  end
end
```

We have already created specific **State** classes, so now we can create our **Order** class. This class will contain all the methods we need, but in fact, they will be executed according to a specific state.

``` ruby
class Order
  attr_accessor :state

  def initialize
    @state = NewState.new(self)
  end
  
  def pay
    state.pay
  end
  
  def next_state
    state.next_state
  end

  def take
    state.take
  end

  def rate(number)
    state.rate(number)
  end

  def status
    state.class.name.gsub('State', '')
  end
end
```

The order created in this way allows the **Order** to use only those methods that are allowed in a given **State**.

``` ruby
order = Order.new
order.status # => "New"

order.take # => RuntimeError: You cannot use this method on this state

order.pay # => Payment has been accepted
order.status # => InProgress

order.next_state # => Order is ready to take
order.take # => The order has been taken

order.take # => RuntimeError: You cannot use this method on this state
order.rate(5) # => "You rated the order to: 5"
```

### Pros:
- Organize the code related to particular states into separate classes (SRP)
- Introduce new states without changing existing state classes or the context (OCP)
- Simplify the code of the context by eliminating bulky state machine conditionals

### Cons:
- Applying the pattern can be overkill if a state machine has only a few states or rarely changes

### More reading
- https://refactoring.guru/design-patterns/state
- https://medium.com/@kroolar/design-patterns-in-ruby-state-541305f5e948
