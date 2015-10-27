# Effective and Clean Java Code? Tips and Tricks from the Real World

> "Now, *every* company *is* a software company." -Forbes

## Encapsulation with JPA and Domain Driven Design

* Don't fight the framework
* Don't make IDs final, just omit the setter
* Always use factory methods
* Make value types immutable (like `OrderNumber`)
* Use different representations for printing values to logs vs. for the user
* Complaint: Too much boilerplate. He suggests we use *Immutables*, which helps build custom types.


```java
@Entity
public class Order implements Serializable
{
  @Id
  @GeneratedValue
  private Long id;

  private OrderNumber orderNumber;

  protected Order() {}

  // Provide a factory method
  public static Order of()
  {
    return new Order();
  }

  public Long getId()
  {
    return id;
  }

}
```

### OrderNumber class

* Using Guava to check arguments
* Properly implement `equals` and `hashcode`

```java
public class OrderNumber implements Serializable
{
  private final int value;

  private OrderNumber(int value)
  {
    this.value = value  
  }

  pubic static OrderNumber of(int value)
  {
    checkArgument(value > 0);
    return new OrderNumber(value);
  }

}
```

### Using *Immutables*

* Plugs into Maven
* Always creates a builder, but can be configured to do Factory methods too.
* Automatically provides you `equals`, `hashcode`, and toString
* If you make it `Formattable`, need to provide an implementation
* If you use standard JPA, you need to use attribute converters so it knows how to save your object.

```java
@Value.Immutable
public abstract class OrderNumber implements Serializable, Formattable
{
  public abstract int value();
}
```

## Other recommendations

* If you are returning a member object (like `OrderItems`), be sure to return a defensive copy.
* Can create a separate class to represent lists or aggregates. Then you can put methods that treat the list as a whole there, like `getQuantity` or `getQuantityByProduct`
* If you are using a list of elements in JPA, annotate with `@ElementCollection`
* When naming methods, don't mention anything implementation related (i.e. `getQuantityByProduct` vs `getQuantityByProductMap`)
* Lambda for this:
```java
values.stream()
  .collection(Collectors.groupingBy())...
```
* Enums can be used for conversion (passed into `.map()` in a Lambda)
```java
enum operation implements Function<orderItem, OrderItem>
{
  ADD {
    @Override
    public OrderItem apply(OrderItem item)
    {
      return item;
    }
  }
}
```
Could also use abstract classes

## JavaSlang

* Library for functional code for Java 8+
* Tuples
* Class called `Try` to call code that throws exceptions in your Lambdas. Normally can't call code that throws checked exceptions
