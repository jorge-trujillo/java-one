# API Design with Java 8 Lambdas and Streams

Focus on lessons learned when building the Java 8 APIs by Brian  Goetz and Stuart Marks, who work on the Java Platform.

Will discuss Java 8 features, when to use and not to use features.

## Lambda Expressions

* Compact way to express "code as data"
* Allows passing *behavior* through an API, not just values
* APIs previously used anonymous inner classes to pass code as data
* Leads to more expressive code and less error prone

### Example: ThreadLocal

* Template Method Pattern is used to override default value. You have to subclass ThreadLocal, which is onerous.
  * Subclass and override `initialValue()` method
* Java 8: use lambda to "plug in" initialization function into the right place.

```java
// NEW

static ThreadLocal<Integer> threadId =
  ThreadLocal .witInitial(() -> computeNextId());
```

### Example: Multi-Valued Map

* Creating a map where the values are a multi-value set: `Map<Key, List<Value>>`
* To add you have to:
  * Check if the key is there
  * If not, create an empty list and add it to the map
* New approach makes use of a Lambda and `computeIfabsent`

```java
// NEW

map.computeIfAbsent(key, k -> new ArrayList<>())
  .add(newvalue);
```

**Benefits**

* `Map.computeIfabsent()`
  * If key is absent, computes a value, puts it in the map and returns it
  * If it is present, return the value
* Cleaner, lazy execution and more readable

### Example: Sorting Collections

* Existing sorts:
  * `Collections.sort(List)` or `Collections..sort(List, Comparator)`
* Wanted to support common cases
  * Sort by field or property
  * Reversed-order sort
* List of potential sorts would be very lonn
* Level of abstraction was too low, need to work at the `Comparator` level

#### Comparators

* Comparator is just a function that returns <0, 0, or >0 depending on the values

Base examaple:
```java

class Student
{
  public String getLastName() { ... }
  public String getFirstName() { ... }
}

// NEW
(s1, s2) -> s1.getLastName().compareTo(s2.getLastName())

// NEW AND IMPROVED
Comparator.comparing(Student::getLastName)
Comparator.comparingInt(Student::getScore)

// NULL HANDLING OR 2-LEVEL SORTING

static <T> COmparator<T> nullsFirst(Comparator<T> original)
{
    return (t1, t2) -> {
      if (t1 == null)
        return -1;
        ...
    }
}
// Nulls first
Comparator<Student> studentsByFirstNameNullsFirst =
  Comparator.comparing(Students::getFirstName, nullsFirst(naturalOrder()));

// 2-LEVEL
Comparator<Student> studentsByLastNameThenFirstName =
  Comparator.comparing(Student::getLastName)
    .thenComparing(Student::getFirstName);
```

**Lessons**

* Some APIs have combinatorial explosion of complexity.
* Break down the problem into smaller features that can be composed
  * Write higher-order functions to do the composition
  * Allow the user to plug in logic using Lambdas


## Streams

* Might want `forEach` on collections, but considered that you may want filtering, sorting, mapping, etc.
* Explosion of complexity again
* Ended up applying streams to everything
* Do you return a collection or a stream for aggregates?
  * It doesn't matter, caller can convert between them easily.
  * Few times where you should think about preference

### Stream vs. Collection

* Stream instead of a collection if:
  * Creating the collection is expensive
  * Cheaper to produce elements lazily on demand
  * Caller needs only a subset of the elements
* Collection instead of a Stream
 * Snapshot of the data* Caller needs to traverse multiple times or in different directions

### How to Return a Stream

* Zero elements: `Stream.empty()`
* Fixed number of elements: `Stream.of(e1, e2, e3)`
* If you have a collection: `stream()`
  * Caller cannot use that stream to modify your internal collection
* If you have an array: `Arrays.stream(array)`

**From an Iterator**

```java

// if size unknown
StreamSupport.stream(
  Spliterators.spliteratorUnknownSize(iterator, o), false);

// if size known
StreamSupport.stream(
  Spliterators.spliterator(iterator, size, 0), false);

```

Writing your own Spliterator:
```java

boolean tryAdvance(Consumer<Onbject> consumer)
{
  Object obj = gettheNextObject();
  if (obj == null)
    return false;
  consumer.accept(obj);
  return true;
}

```
* Writing an easy spliterator may be easier than writing your own iterator
* For improved sequential performance, implement `forEachRemaining()`
* For better parallel scaling, implement `trySplit()`
* Consider also primitive specializations for int, long, double

**Why Spliterator**

* Iterator not quite the right approach
  * Two method calls per element, `hasNext()` and `next()`
  * Often interact in subtle ways
  * Need to guard against unusual call order

* Spliterator
  * One method: `tryAdvance()`
  * A better iterator than Iterator, even for sequential processing
* All collections have been retrofitted with Spliterators to enable parallelism

## Optional

* Primary use is:
> Optional is intended to provide a *limited* mechanism for library method return types where there is a clear need to represent "no result," and where using null for that is overwhelmingly *likely to cause errors*.

* Use it as a return value, when absence of a value is an *expected* result
  * as opposed to an exceptional result
  * example: `findFirst()` or similar method
* A method returning Optional should **NEVER** return null!
* Terminology: prefer "empty Optional" over "Optional containing null"

### When to use

* Method chaining
  * Returning an Optional allos caller to chain methods safely
  * `orElse()` returns value if present, else substitutes a default value. Avoid doing `orElse(null)` if at all possible.
  * `orElseGet()` returns a value if present, else calls a lambda to generate the value.
  * `orElseThrow()` throws an exception if the value is absent
  * `get()` immediately throws `NoSuchElementException` if there is no value. Probably don't want to use this!

* Don't use for:
  * Method arguments
  * Object fields
  * In a collection
* It seems this should work, but adds clutter.
* It is *not* a goal to get rid of nulls everywhere
* Sometimes its ok to use null
  * a private field with null as a sentinel
  * as a method argument. You should be checking arguments still.

## Interfaces & Default Methods

* Primary use case: evolving an existing interfaces
* Before Java 8, you would never be able to change an interface, since adding methods would cause `AbstractMethodError` if the implementing class was missing it.
* Default methods are ordinary virtual methods and can be overridden
* For example: In Java 8, they added a default implementation for `remove()` on Iterators

* Not meant as a replacement for Abstract classes.
* Classes still have:
  * state (fields)
  * constructors (allowing control over instance creation)
  * protected methods
