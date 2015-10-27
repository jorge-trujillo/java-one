# 12-Factor App and Modern Java Deployment

* Presentation by the JVM Platform owner at Heroku. Collection of principles and best practices
* Moving away from WAR files, app servers, hot deploying
* Moving toward JAR files, microservices, continuous deploy, Heroku/AWS/etc
* **Get rid of WAR files**

Links:
* http://12factor.net
* http://jkutner.github.io
Slides:
* http://bit.ly/1PSRxcm
* http://www.slideshare.net/jkutner/javaone-2105-12-factor-app

## High-level goals

* Immutable
* Ephemeral
* Declarative
* Automated

## 12 Factors

### Codebase

* You should use version control
* Leverage that one repository and push to multiple environments
* If you have a sub-module, it should be a separate git project with an independent commit history

### Dependencies

* Explicitly declared and isolated
* Don't check JAR files into git
  * Use Bintray
* Never rely on implicit existence of system-wide packages

### Configuration

* Credentials to Amazon
* Database or memcached handles
* Per-deploy values
* Should be strictly separated from your code.
  * Don't check passwords into git.
  * Drivers are security and portability, since it makes it harder to stand up new environments.
* Where does it belong? In the environment in environment variables. Could use service discovery too I think.

### Backing services

* Database, Postgres, Memcached, etc.
* Should be treated as attachable resources as a single URL. Will not work for MongoDB!

### Build, release and return

* App should be deloyed by 3 steps:
  1. Build
  1. Release: Combine build and combine with environment config, creating a release image
  1. Run: Run processes based on that image

### Processes

* Processes should be stateless, so that if they crash no info is lost.
* Don't use sticky sessions!

### Port binding

* Self-contained, application should bind to the port itself
* Don't depend on Tomcat, it should be embedded into app. This is the direction of the industry.

### Concurrency

* Lots of frameworks, RXJava, etc.
* Be able to scale out too (web tier vs. background tier vs. app tier)

### Disposability

* Quick to start up (sub 1-minute), graceful to shut down, resilient to failure
* Servers should be cattle, not pets
* Decoupled from external infrastructure

### Dev/Prod Parity

* Dev environment should be identical to prod environment and every one in between
* Parity helps with reproducibility and disposability

### Logs

* Treat logs as events, not as files

### Admin Processes

* Should run as isolated processes
* Should never need to log into prod to run a one-off admin job

## Next steps

* Create a bintray.com account
* Measure your app start-up time
