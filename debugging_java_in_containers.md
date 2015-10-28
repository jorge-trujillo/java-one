# Debugging Java Apps in Containers: No Heavy Welding Gear Required

* Can still use standard Java tooling
* Resources:
  * http://www.joyent.com/blog/linux-performance-analysis-and-tools-brendan-greg-s-talk-at-scale-11x


## Using Dockerfiles

* Could build a base image with your application, and have a 2nd image overriding the CMD to add debug options.
* Treat Docker image as a remote server, open the port using `-p`
* Attach through the port using IDE
* Can also attach using exec:
```bash
docker ps -a
docker exec -it <container> /bin/bash
```

## Using Kitematic

* Just a GUI for docker CLI commands

## Containers

* Minimal OS, limited resourecs
* JVM doesn't always respect resource limits
  * cgroup/namespace awareness
  * forkjoin pool setting may have to be overridden
* Constant restarts and reallocations

### Cloud considerations

* Everything is on the network, so keep latency considerations in mind.
* Noisy neighbors, with contention for network with other containers on the network.
* No writing to local disk
  * Typically ephemeral

## Case Study notes

* A lot of talk about using Mesos/Marathon to manage how much resources were available to containers
  * Need to allow -Xmx space + *overhead* for memory
  * Can do around 10-20% for overhead

* /dev/random is a blocking call on Linux. Not so good on containers
  * Solution: `-Djava.security.egd=file:/dev/urandom`

* Java can cache DNS
  * Can use command-line to set TTL: `-Dsun.net.inetaddr.ttl=10` (10 seconds)

## Logging and Metrics

* Should log and ID for all requests
* Talked about using Zipkin for monitoring performance across the stack
