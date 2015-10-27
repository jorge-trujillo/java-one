# Programming with Lambdas

Code in http://www.agiledeveloper.com

## Example 1: Iterating through a list of numbers, using an internal iterator

With type and no ceremony
```java
List<Integer> numbers = Arrays.asList(1,2,3,4,5);
numbers.forEach(
  (Integer it) -> System.out.println(it));
```

Removing the type
```java
List<Integer> numbers = Arrays.asList(1,2,3,4,5);
numbers.forEach(it -> System.out.println(it));
```

Further simplification using method reference.
```java
List<Integer> numbers = Arrays.asList(1,2,3,4,5);
numbers.forEach(System.out::println);
```

## Example 2: Total the values in the collection

```java
List<Integer> numbers = Arrays.asList(1,2,3,4,5);
System.out.println(
  numbers.stream()
    .mapToInt(e -> e * 2)
    .sum());
```

Declarative code:
* Specify what and not how
* No explicit mutability
* Described as talking to a responsible adult vs. a toddler (for imperative)

Functional programming uses higher-order functions, which can take other functions as parameters as well.
* In the previous example `mapToInt()` takes a function as a param

## Example 3: Composition and pipelining. Find the double of the first even number greater than 3

Declarative code shows the issue with the previous imperative logic

```java
List<Integer> numbers = Arrays.asList(1,2,3,5,4,6,7,8);
System.out.println(
  numbers.stream()
    .filter(e -> e > 3)
    .filter(e -> e % 2 == 0)
    .map(e -> e * 2)
    .findFirst());
```

This returns an Optional
Issues:
* What if it were empty, or didn't have required conditions? Can use `orElse(0)`


### Performance of this example

* Imperative did 8 operations, accounting for short-circuiting
* Intermediate functions (`filter`, `map`, etc) are lazy evaluated, and the terminal function (`findFirst()`) triggers the evaluation.
* Operation is done for every element, and terminates after the `findFirst()` op finds a suitable element.
* Completes in 4 operations
* Easy to parallelize

## Example 4: Getting prices from the Internet

```java
public static void findFunctional(Stream<String> symbols)
{
  symbols.parallel()
    .map(StockUtil::getPrice
    .filter(StockUtil.isPriceLessThan(5000))
    .reduce(dummy, StockUtil::pickHigh));
}
```
