# GC Tuning Confessions of a Performance Engineer

## Performance Engineering

* Should start to think about performance as an NFR while designing the application.
* Should be careful with metrics being captured.
  * Number of events, 99th percentile, median, min and max
  * Can give insight into GC issues

## GC Basics

* Need to pick two: Minimum GC time, mininum number of GC collections, min footprint
* Throughput and latency are the two main drivers for GC algorithms
* Single generation GC doesn't scale well, need to go to a generational GC
  * Most allocations happen in Eden, survivor spaces, and finally promote to old generation
  * Parallel worker threads
* Latency sensitive: Concurrency reduces latency
* All collectors fall back to a full, "stop the world" collection
  * Can avoid with G1

## Different collectors

* Serial collector is single threaded, and least complex. Stops the world and fully marks, sweeps, compacts and disposes.
* Throughput collector: Is similar to serial, but multi-threaded.
* CMS collector: Fully multi-threaded, and only for old gen. Concurrently marked, but fully swept.
* G1 to be covered later

## What triggers a full GC
* Promotion failures: Object makes it way through Eden, S0, S1 and then cannot be promoted to old gen.
* For throughput, the mark, sweep and compact would happen in parallel.
* For CMS, old gen available memory is maintained by free lists.
  * If an object can't be promoted, you have another promotion failure.

## G1 Collector and Incremental Compaction
* Regionalized heap configuration
* CMS only pertains to old gen, G1 applies to entire heap.
* When heap crosses a certain threshold, the marking cycle starts
* G1 collects garbage regions first. Easy collections happen first, more expensive operations happen later.

## Recommendations
* Be careful with too much tuning, may restrict G1 adaptability
* Set heap large enough to fit live data set
* Increase the `G1ReservePercent` if to space surivors are triggering evacuation failures

## Fragmentation in G1
* G1 is designed to absorb some Fragmentation
* Default is 5%
* Each region in G1 has a liveness threshold. It is 85%, so if more than 85% of data is live, it is too expensive. This is tunable.

## Logs
* G1 GC logs are very detailed
* First line says what kind of pause it was and what space it affects (young, old)
* *humongous reclaim* is a new section, talkes about objects that are not reachable
* Overhead is a measure of the frequency of stop the world GC events
* GC elapsed time indicates the amount of time it takes to execute stop-the-world events

## Summary

* Most allocations in Eden
* CMS maintains free-list for old gen availability

### Tunables

* G1: Lots of tunables and defaults
  * Pause time goal, heap size, max and min nursery, concurrent and parallel threads
  * Marking threshold, number of mixed GCs
  * Understand `G1HeapRegionSize`, can't range from 1MB to 32MB
  * Setting overly aggressive pause time goals will increase GC overhead
* Best practice for starting baseline: Start with some lower number (~500 ms pause time goal, maybe even a second)

More on defaults here: http://www.oracle.com/technetwork/articles/java/g1gc-1984535.html
