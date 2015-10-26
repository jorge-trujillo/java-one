[[Using Docker and Kubernetes to deploy Java applications]]

All materials here: https://github.com/javaee-samples/docker-java

## About Docker

* Most popular containerization framework
* Package Once Deploy Anywhere (PODA)

### Advantages

* Immutability
* Reproducibility - Everyone is running the same image and same environment
* Isolation
* Faster deployments
* Portability
* Snapshotting
* Security sandbox - Cannot easily affect rest of applications
* Limit resource iusage
* Simplified dependency - All dependencies are packaged
* Sharing - Much easier to share completed images

### Comparing to VMs

* No Guest OS layer
* Can use a regular OS, Docker Engine basically replaces Hypervisor

### Getting started

There is an extensive tutorial in his materials.

### Other notes

* What is the function of `EXPOSE`? It's mostly documentation, but useful when doing Docker container linking using the `--link` keyword.

### Docker Machine

* Can use to create a Docker Host on a Mac.
```bash
docker-machine create --driver=virtualbox myhost
```
* Only required for Macs or other non-Linux OSs
* Openstack can be a provider for Docker Machine

### Docker Compose

* Used for applications that require multiple containers
* Can specify image name and environmment variables
* Not recommended for production yet

** Volume Mounting **

* Containers are supposed to not have state. State and data should be stored in volumes.
* Best practice is to store data in data volume containers.

### Docker Swarm

* Native clustering, allows you to target many hosts as one Docker host.
* Fully integrated with Docker Machine, can add hosts to Swarm
* Partially integrated with Compose
* Direct competitor of Kubernetes

** Creating a Swarm **

* Swarm Master is the administration entry point, and currently a SPOF
* A token ID is created when you create a swarm. It is provided only 1 time.

More here: https://github.com/javaee-samples/docker-java/blob/master/chapters/docker-swarm.adoc


## Kubernetes

* OpenShift 3 uses Docker and Kubernetes as its core tech
* Orchestration system for Docker containers
* Mesos is another option
* Can give desired state in a declarative way
* Dynamic scale up and scale down

### Concepts

**Pod**: Collocated group of containers that store an IP and storage volume
* Each Pod is given an IP address
* Typically Pod -> Container is a 1:1 mapping
* This IP address for a Pod is ephemeral
**Label**: Used to organze and select a group of obkects
**Service**: Single stable name for a Pod, acts as aload balancer
* etcd is a distributable, watchable registry. Service discovery agent
**Replication Controller**: Wraps a pod, manages the lifecycle of a pod and ensures the needed number are running.

### Architecture

* Single master, but there are discussions going on to remedy this. Only used for administration.
* There is the concept of a Kubernetes provider, could be Google Compute, Vagrant, Openstack?
  * num_minions: Number of nodes

### Running
* `kubectl` is script used to control the cluster
* `kubectl get pods` or `kubectl get minions`
* Replication controller allows you to specify how many replicas need to run.
  * Can increase the number of replicas while running for scaling.
* Has new concept of health checks that can be used to assess the health of your container. Can be used to bounce or restart.
* There is a "load balancer" component in Kubernetes that can be front-ended
