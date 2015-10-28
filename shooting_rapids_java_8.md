# Shooting the Rapids: Getting the Best from Java 8 Streams

Talk about benchmarking and using Java 8 parallel streams.

## Intro: lambdas, streams and logfile processing

* Example: Processing a GC logfile that parses lines for GC time, and adds it up
* Old school implementation as reference point
* Source of a stream is a spliterator
  * In a sequential stream, performs a lot like a regular iterator
  * Will get split into sub-arrays in the case of parallel streams
* Set up a stream implementation of the parsing code for comparison
  * Old school: 80200ms
  * Sequential: 25800ms
* Stream code is faster because operations are fused

Example code:

```java
logFileReader.lines()
  .paralell()
  .map(stoppedTimePattern::matcher)
  .filter(Matcher::find)
  .map(matched -> matcher.group(1))
  .mapToDouble(Double::parseDouble)
  .summaryStatistics();
```

* Forkjoin works best if you are CPU bound. If you are IO bound, then you won't get as big a benefit.


## Optimizing stream sources

* Some sources split much better than others
  * LinkedList vs. ArrayList (easy to find mid-point in an ArrayList)
* Streaming IO is bad, as it kills a lot of the benefit from going parallel
* For a file, could read it into memory
  * Split based on lines
  * Can go to the middle of the data, find line endpoint, and split that way.
  * It's in JDK 9: `FileChannelLinesSpliterator`


## Tragedy of the Commons

* There is a finite amount of hardware
  * It may be in the best interests to grab it all, but if all callers do so the end result may be suboptimal
* One pool of Forkjoin threads, so you may be slowing down overall performance.
  * Can configure it or create your own pool
```java
ForkJoinPool owrOwnPool = newForkJoinPool();
owrOwnpool.invoke(
  () -> stream.parallel()
);

```
* Be careful with `.parallel()` in Tomcat, as you have threaded stuff creating many threads.

## Justifying the Overhead

* Use when the task is recursively decomposable
  * Subtasks for the data segment must be independent
  * Source must be well-splitting
* Must have enough hardware capacity for other processes going on
* Expensive to set up, so **time gain must be greater than the cost of the setup process**.

Guide: http://gee.cs.oswego.edu/dl/html/StreamParallelGuidance.html

* CPNQ performance model:
  * C - number of submitters
  * P - number of CPUs
  * N - number of elements
  * Q - cost of the operation

* Need to amortize setup costs:
  * N*Q needs to be large
  * N should be >10,000 elements

* Don't have too many threads, as they cause frequent handoffs
* It costs ~80,000 cycles to handoff data between threads
