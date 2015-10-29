# Java One 2015 Field Report

This repository contains high-level notes of all notable Java One sessions I attended.

I have indented what I think where the top sessions, and highlighted the topic for easy browsing. If you are a Java/JVM dev, definitely take a look at some of the sessions on Lambdas, especially where they talk about being careful with parallel streams, as they can cause issues.


## Day 1

Day 1 was mostly about the Java Keynote, plus a few sessions.

- [**Building Nanoservices with Java EE**](building_nanoservices.md): Not all that applicable to what we're doing, but there were some interesting notes on cost and reasons for doing Microservices.

## Day 2

> - [**DEVOPS - Docker + Kubernetes**](docker_kubernetes.md): More introductory than I hoped, but good overview of using Docker in a clustered environment using Google's Kubernetes.

- [**JAVA - Java GC Tuning Confessions**](java_gc_tuning.md): Deep look at the different GC collectors, what's tunable, and good details on G1 (new collector for Java 8).

- [**GROOVY - Groovy + REST**](groovy_rest.md): High-level session on different REST APIs available on Groovy. Everything except for Grails basically.

- [**DEVOPS - Docker Orchestration for Java**](docker_java_orchestration.md): Talk by Patrick Chanezon from Docker on different available orchestration systems out there for Docker containers.

> - [**JAVA - Programming with Lambdas**](programming_with_lambdas.md): Good talk about using Lambdas to simplify code, make it more readable, and improve performance.

- [**JAVA - Tips and Tricks with IntelliJ**](intellij_tips_tricks.md): A look at some of the new features in IntelliJ 15, and interesting short-cuts

> - [**JAVA - Intro to Thread Dumps and Using Samurai**](intro_to_threadumps.md): Interesting look and debugging slowdowns and deadlocks using thread dumps, and the Samurai tool to visualize them.

## Day 3

- [**DEVOPS - Deploying J2EE 7 Microservices to the Cloud**](deploying_microservice_cloud.md): Tutorial-type walk-through of building a small service using J2EE and JCache, testing it using Payora, and then deploying it to Amazon Elastic Beanstalk.

- [**JAVA - Speech Recognition Using Java**](speech_recognition_java.md): Code examples and demo of using CMU Sphinx for speech recognition.

- [**JAVA - Effective and Clean Java Code**](effective_clean_java.md): Talk about Domain Driven Design and writing clean, well encapsulated code. Some interesting tidbits but very JPA/relational focused.

- [**DEVOPS - Logging and Metrics for Microservices Architectures**](logging_and_metrics.md): Good discussion about capturing both logs and metrics, and how to store them and make them searchable for debugging and reporting on business and application health.

> - [**DEVOPS - 12-Factor App and Modern Java App Deployment**](modern_java_deployment.md): Solidly good talk on what you should be shooting for when deploying modern applications. Most interesting is that he recommends completely moving away from app servers.

## Day 4

> - [**JAVA - API Design with Java 8 Lambdas and Streams**](api_design_java_8.md): Focus on lessons learned when building the Java 8 APIs by Brian  Goetz and Stuart Marks, who work on the Java Platform.

- [**DEVOPS - Debugging Java Apps in Containers**](debugging_java_in_containers.md): Highl-level talk and case studies about debugging issues in highly-distributed environments.

- [**DEVOPS - CI/CD Antipatterns**](cd_antipatterns.md): Basic session on how to manage stability and continuous delivery.

- [**DEVOPS - Low Latency Apps with the JVM**](low_latency_java.md): Crazy look at the world of low latency and high frequency trading. Interesting but not very applicable.

> - [**JAVA - Shooting the Rapids: Getting the Best from Java 8 Streams**](shooting_rapids_java_8.md): Explanation of performance considerations for parallel streams, and when to use them or, more importantly, avoid them.

## Day 5

- [**JAVA - Big Data Engineering: The Hitchhiker's Guide**](big_data_engineering.md): Survey of current Big Data solutions (Hadoop, Storm, Kafka, Spark) and use cases for them.

> - [**JAVA - A Few Hidden Treasures in Java 8**](hidden_treasures_java_8.md): Another great talk by Venkat S. on really useful additions to Java 8. Recommended, and full examples are on his site.
