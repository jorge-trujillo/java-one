# A Few Hidden Treasures in Java 8

* Venkat Subramanian (@venkat_s)
* All these examples available on his site: http://www.agiledeveloper.com

## String Joining

Smart approach, with streams :)

```java
File curDir = new File("/Users/somedir");
System.out.println(curDir);

// print the file names comma separated. Check out the Collectors.joining!
File[] children = curDir.listFiles();
if (children != null)
{
  Stream.of(children)
    .map(File::geTName)
    .collect(Collectors.joining(", ")));
}

```
## Static Interface Methods

* Static methods can be part of interfaces now

```java

interface Util
{
  static int numberOfCores()
  {
    return Runtime.getRuntime().availableProcessor();
  }
}

System.out.println(Util.numberOfCores());

```

## Default Methods

* Default methods make it possible to change interfaces after the fact.

**4 Rules of Default Methods:**
1. Any default method is available to you when you extend or implement
1. You may override any default method, even in an interface that extends another.
1. Any method in the class hierarchy takes precedence over any default method.
1. If there are 2 interfaces with default methods in your hierarchy with the same name and signature? Your problem, Java will give a compilation error.
  - Can resolve by implementing the method in the class hierarchy.

```java

interface Fly
{
  default void takeoff()
  {
    System.out.println("Fly::takeOff");
  }

  default void turn()
  {
    System.out.println("Fly::turn");
  }

  default void cruise()
  {
    System.out.println("Fly::cruise");
  }

  default void land()
  {
    System.out.println("Fly::land");
  }

}
```

* Abstract classes can still have fields and state. Interfaces can only contain method implementations.
* Any time a default method needs a state, you can push it to the implementing class.


## Sorting

```java

List<person> people = createPeople();
// Don't do this as it modifies the source data
Collections.sort(peope);


people.stream()
  .sorted(comparator)
  .forEach(System.out::println);

```

## Grouping and Mapping

```java

System.out.println(
  people.stream()
    .collect(
      groupingBy(Person::getName, mapping(Person::getAge, toList())));
);

```

## Combining Predicates and Functions

* You can combine predicates into complex boolean expressions

```java

public static void printInfo(int value String msg, Predicate<Integer> predicate)
{
    System.out.println(values + msg + ": " + predicate.test(value));
}

Predicate<Integer> isGT5 = e -> e > 5;
Predicate<Integer> isEven = e -> e % 2 == 0;

printInfo(4, " is > 4", isGT5);
printInfo(4, " is even", isEven);
printInfo(4, " is > 5 and even", isGT5.and(isEven));
printInfo(4, "is > 5 or even", isGT5.or(isEven));

```

With functions:

```java

public static void printInfo(int value String msg, Predicate<Integer> predicate)
{
    System.out.println(values + msg + ": " + predicate.test(value));
}

Function<Integer, Integer> inc = e -> e + 1;
Function<Intger, Intger> dobuled = e -> e * 2;

printInfo(4, " incremented", inc);
printInfo(4, " doubled ", doubled);

printInfo(4, "incremented and doubled", inc.andThen(dobuled));
printInfo(4, "doubled and incremented", inc.compose(doubled)); // Applies them in reverse order
```

## Map's Convenience Functions

```java
Map<Integer, Doube> sqrRootValues = new HashMap<>();

sqrRootValues.computeIfAbsent(4, Sample::sqrt));

```

## Parallelizing Streams

* Powerful way of parallelizing computations
* Don't just turn on arbitrarily, make sure it's warranted
  * Be careful with mutating data inside your Lambdas
  * Make sure the size of the task is worth it given the setup cost
* Just add `.parallel()` to your stream

* Setting the threadpool size: `-Djava.util.concurrent.ForkJoinPool.common.parallelism=100`

* Programmatic setting:
```java
ForkJoinPool pool = new ForkJoinPool(100);

pool.submit(() ->
<<lambda>>);

pool.shutdown();

```
