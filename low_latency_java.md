# Low Latency Computing with the JVM


## What is latency

* Code will be doing stuff and then waiting
* Total response time = service time + time waiting
* Serialization is everywhere (resource point of contention)
  * We live in a bounded compute environment
  * Little's Law: I can tell you your throughput based on points of serialization
  * Amdahl's Law: States how much CPU is usable based on points of serialization.

## What is low latency

* Latency that is not noticeable by a human
  * Generally around 50ms
* Low latency for trading systems is faster than everyone else
  * Sub 1-ms
* Less latency is perceived as better QoS
* In an auction scenario, 2nd place is not valuable

### HFT

* Trying to get down to 10s of microseconds
* High throughput
* 1ms can be worth $100m
* Not really a place for Java, more C/C++ and specialized hardware

## Source of latency

* Speed of light
* Hardware sharing (scheduling)
* OS maintenance
* KVM safe-pointing
* Hardware works in blocks of data, if your data fits into regular sized block things will work well.
  * CPU: word size, cache line size, internal buses
  * OS: pages
  * Network: MTU
  * Disk: sectors
* A lot of strategies involve bypassing the kernel to minimize context switching and latency

### JVM Latency

* Safe-pointing: Called for when the JVM has to perform some maintenance
  * Parks application threads
  * Rogue thread could delay the safe-pointing from completing
* Safe-pointing is called for GC, lock deflation, code cache maintenance, HotSpot optimization
* Can reduce latency by reducing the number of threads

### Hardware

* Want to reduce the hits to L3 caches
* Access times range from L1/L2 cache (single digit ns), to disk reads (10s of ms) to network access (hundreds of ms)
* Intel added a feature where you can partition the L3 to reduce contention
* CPUs are getting smarter and processing instructions, even if clock rates aren't climbing as quickly anymore

### Memory pressure

* Predictability helps the CPU remain busy
* Java heap is not predictable, and can idle the CPU
* By addressing issue, they went from 40k TPS to 7M TPS
* Look at allocation rates by analyzing GC logs
  * High rates mean you are filling your caches with garbage

### Memory layout

* Deep structures are slower than shallow structures
* Java objects form an undisciplined graph
* Want to write things in a predictable way so CPU prefetchers can work appropriately
* Java 7 added compressed OOPS, for compressing 64-bit references to 32-bit

## GC

* Want ultra-low GC. Not only fast collections, but not run them at all
* Have a budget of garbage per hour. Target is under 1GB per hour
* Large Eden means you can even have no minor collections
* G1 is not deemed good for low latency as it can have a lot of overhead

## Network

* Heavy use of binary formats, which speed up network transfers of objects, makes serialized form smaller, faster to write to disk, etc.
