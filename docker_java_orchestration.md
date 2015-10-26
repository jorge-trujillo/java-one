# Docker Orchestration: Welcome to the Jungle!

Talk by Patrick Chanezon on different available orchestration systems out there for Docker containers.

## Cloud Market

* Google, Amazon and Microsoft are giants in public Cloud
* VMWare is a big player in private Cloud

## Linux Containers

* Core OS, RH Atomic, Ubuntu Core are container-focused OSs
* Docker runs on these

## Tools

* Review on *docker-machine* and *docker-compose*. See notes on Docker + Kubernetes for more.
* Docker offers the Trusted Registry product, which you can install behind the firewall.

## Swarm + Others

* Sits in front of your cluster, has the same Docker API
* Docker Swarm doesn't have the concept of "service" like Kubernetes.
  * *Interlock* can be used to regenerate configuration for HAProxy in an automated way
* Project Orca is an enterprise offering for running containers
* Tutum lets you bring your own infrastructure and run containers on it.

## Orchestration

* Swarm scheduler allows for constraints on memory, CPU, network
* Integrated with Machine and Compose
* Mesos + Docker Swarm is an option
* Mesos + Marathon
* Swarm is ready for 1.0. Network overlays used for connecting multi-container apps
* Parallel scheduling, >1k nodes
* Docker Engine and Swarm are coming closer

### Mesos

* Apache project, scalable to 10k+ nodes
* Mesos master manages resources, and then you need a scheduler

### Kubernetes

* Adds new higher-level abstractions: Pod, Replication Controller, Service
* Pod is the smallest deployment unit
* Then can assign tags to them, and control how many with a replication controller

### Cloud Foundry

* Based on build packs
* Need to adhere to certain build-packs, and focus on the code
* Very complex from an ops perspective
* New sub-project called *Diego* is their orchestration tool for Docker

### Joyent Triton

* Cloud provider based on Sun Solaris code
* The whole datacenter exposed as one Docker endpoint

### Demo

* Used a plain Java app
* Passed in MongoDB URI as an environment variable
* Did not address how to handle multiple hosts for MongoDB
* Swarm can use Consul for its bookkeeping.
