# Deploying Microservices to Cloud Environments

This session makes use of "Payara", which is some vendor app runtime, but we can ignore it since Tomcat would be an exact substitute.

Docker File Example: https://github.com/smillidge/JavaOne-Docker-Example
Payara site: http://www.payara.fish

## Why Java EE?

* Lightweight and rapid to develop
* Produces smaller WARs

## Building a Microservice

* Text cache using JAX-RS and JCache
* Added a GET and PUT method for resources. Not Spring so the class is not a `@Controller`...

## More on JCache

* Standard API for caching (JSR107)
* API and CDI binding
* Supported by many caching providers (like Hazelcast)
* javax.jcache
* This implementation uses Hazelcast, which is effectively a distributed hashmap.

### Annotations

- @CacheResult
- @CachePut
- @CacheRemove
- @CacheRemoveAll

## More on Payara

* Payara "micro JAR" is 60MB, and can be used as a full app server
* Only includes EE web profile and concurrency profile, not the full J2EE profile

## Packaging as a Docker container

* Docker images are built in layers: OS -> Java -> Payara Micro -> WAR file
* Docker CI process:
  * Java Tests -> Package WAR -> Create Docker Image -> Test Image -> Push to Repo -> Pull from Repo -> Deploy to Prod
* He extended an existing image on Docker Hub
```
FROM payaradocker/j1-payara-micro
```
* Just ADDed WAR file
* Can use the `docker history IMAGE` command to see how much size each command adds. Very cool.

## Deploying to Elastic Beanstalk

* Elastic Beanstalk is an Amazon AWS service that allows you to run Docker images on the Cloud
* Can also support WAR files on Tomcat
* Can set rules on levels of traffic for elastic scaling
* Can create a new application
  * Web Application will provide load balancing
  * Can run generic Docker 1.7.1 images
  * Can create application inside a VPC, which is Amazon's term for a subnet
  * Creation takes 6-10 minutes
