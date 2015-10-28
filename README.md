# Java One 2015 Field Report

This repository contains high-level notes of all notable Java One sessions I attended.

## Day 1

Day 1 was mostly about the Java Keynote, plus a few sessions.

- **Building Nanoservices with Java EE**: Not all that applicable to what we're doing, but there were some interesting notes on cost and reasons for doing Microservices.

## Day 2

- **Docker + Kubernetes**: More introductory than I hoped, but good overview of using Docker in a clustered environment using Google's Kubernetes.

- **Java GC Tuning Confessions**: Deep look at the different GC collectors, what's tunable, and good details on G1 (new collector for Java 8).

- **Groovy + REST**: High-level session on different REST APIs available on Groovy. Everything except for Grails basically.

- **Docker Orchestration for Java**: Talk by Patrick Chanezon from Docker on different available orchestration systems out there for Docker containers.

- **Programming with Lambdas**: Good talk about using Lambdas to simplify code, make it more readable, and improve performance.

- **Tips and Tricks with IntelliJ**: A look at some of the new features in IntelliJ 15, and interesting short-cuts

- **Intro to thread dumps and using Samurai**: Interesting look and debugging slowdowns and deadlocks using thread dumps, and the Samurai tool to visualize them.

## Day 3

- **Deploying J2EE 7 microservices to the cloud**: Tutorial-type walk-through of building a small service using J2EE and JCache, testing it using Payora, and then deploying it to Amazon Elastic Beanstalk.

- **Speech Recognition using Java**: Code examples and demo of using CMU Sphinx for speech recognition.

- **Effective and Clean Java Code**: Talk about Domain Driven Design and writing clean, well encapsulated code.

- **Logging and Metrics for Microservices Architectures**: Good discussion about capturing both logs and metrics, and how to store them and make them searchable for debugging and reporting on business and application health.

- **12-Factor App and Modern Java App Deployment**: Solidly good talk on what you should be shooting for when deploying modern applications. Most interesting is that he recommends completely moving away from app servers.

## Day 4

- **API Design with Java 8 Lambdas and Streams**: Focus on lessons learned when building the Java 8 APIs by Brian  Goetz and Stuart Marks, who work on the Java Platform.

- **Debugging Java Apps in Containers**: Highl-level talk and case studies about debugging issues in highly-distributed environments.

- **CI/CD Antipatterns**: Basic session on how to manage stability and continuous delivery.

- **Low Latency Apps with the JVM**: Crazy look at the world of low latency and high frequency trading. Interesting but not very applicable.

- **Shooting the Rapids: Getting the Best from Java 8 Streams**: Explanation of performance considerations for parallel streams, and when to use them or avoid them.
