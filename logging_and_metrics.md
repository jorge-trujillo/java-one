# Don't Fly Blind: Logging and Metrics in Microservices Architectures

Systems can interact in unexpected ways, so good logging is important.

## Logging

* Start with a well-defined logging concept. What data do you want to log and capture.
* Use Thread Contexts/MDCs
```java
ThreadContext.put("loginId", login);
logger.error("Something bad happened!");
ThreadContext.clear();
```

**Layout**:
%-5p: [%X{loginId}] %m%n

**Output**:
ERROR: [John Doe] Somthing bad happened

* This is nice and human-readable, but also want machine-readable with a structured format.
  * JSON Layout

* You may need different QOS for log messages, as some may be longer-lived. Can route them to different appenders.
* Link: https://www.innoq.com/en/blog/per-request-debugging-with-log4j2/

### Correlate logs across systems

* Need a request ID that can be passed through systems
* Then you can search for that ID across all logs, and correlate what happened
* Logstash architecture: inputs -> codecs -> filters -> outputs (ES)
  * `anonymize` filter could be used to remove user-specific info (PII)

### Visualize and enable drill-down

* Kibana is the goto tool
* MDC (Mapped Diagnostic Context) makes it easier to pull out metadata

### Alerting

* Can filter log stream for alerts
* Can use Logstash to filter messages for values, and send alerts using Pager  Duty

### Recommendations

* Logging is for capturing events in the system, and probably should not be used for metrics.
  * Fragile, since log messages become a contract, and can easily be broken.

## Metrics

Looking at the state of your system should be handled by a separate metrics solution.

Types:
* Business metrics: Loans, sales, etc.
* Application metrics: rates, errors, number of connections, etc.
* System metrics: CPU usage, memory, heap

Goal is to be able to see these metrics in one place, so you can correlate events and find problems.

* Can collect metrics during testing, both automated and manual

### Types of Metrics

* Check out DropWizard library for metrics
  * Gauge: instrument that measures a value.
  * Counter: simple incrementing of a value.
  * Meters: measure the rate at which a set of events occur.
  * Histograms: show distribution of values.
  * Timers: histogram over a period of time.
* These metrics should also be collected in a distributed manner
  * measure -> collect -> store -> query & graph
* Graphite and influxDB and recommended for time series storage
* Should bucket data, and aggregate/reduce resolution as data ages.

Can aggregate data and do:
* CEP: Generate new events from analysis of other events
* Anomaly detection
* Alerting

### Dashboards

* **Grafana** for technicians
* **Dashing** can be used for high-level management dashboards

### Push vs. Pull

* Main difference is source vs. producer discovery
* With pull, you could have multiple targets pulling in data
* Pull will require good service discovery, since the consumer needs to know where to get all the metrics.
* Push seems a lot easier
* Grafana and InfluxDB favor Push, Prometheus promotes Pull

### Summary

* Think about what metrics are of importance and expose them
* Consider retetion and data bucketing
* Carefully design dashboards
