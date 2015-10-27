# Introduction to Threadumps and Samurai

Talk by Yusuke Yamamoto, project lead for Twitter4J

You can run into several
1. Unexpected output
1. Exceptions
1. Slowdowns
1. Freezes
1. Deadlocks

Latter 3 issues have to do with multithreading.

## Thread dump

* Snapshot of the JVM at a particular point in time
* IntelliJ has a "thread dump" button
* On Linux or Mac: `kill -3 PID` -> Traditional but not suggested
* Use the jstack command: `jstack PID`
* `jps -l` shows running Java processes and full class names
  * -m : Shows arguments
  * -v : show JVM options
* Can do it in production, small impact and takes 300ms

## Reading a thread dump

* Gives the call stack and thread name, status
* Possible thread statuses are:
  * RUNNABLE: Can run, most cases they are healthy
  * TIMED_WAITING / WAITING: Waiting and in most cases are harmless
  * BLOCKED: Most dangerous status. Call stack will say what object it wants to lock.
* With one snapshot, it is difficult to determine what a thread is doing.
* Take at least 3 dumps, with interval of 1-2 seconds

## Tool to help analysis: Samurai

* GUI tool built on Swing, visualizes thread dumps
* Open source using Apache License
* Shows all threads on the left, and table on the right
* Each cell is a thread, and each column represents a thread dump
  * Running threads are green
  * Idle threads are gray
  * Orange have a lock
  * Red are blocked, trying to get a lock

### Usage:

* Run thread dump once
* Drag file into Samurai
* Also added a feature to take thread dumps from the UI: File -> Local Processes
* Keep running thread dump and appending: `jstack 1234 >> dump_file.txt`
* `java -jar samurai-3.0.jar`
* Compatible with Oracle JDK 1.6+, OpenJDK 1.5+
