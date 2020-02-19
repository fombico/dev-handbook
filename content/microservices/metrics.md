---
title: "Metrics"
date: 2020-02-18T21:21:50-05:00
draft: true
---

---

## Introduction

This section covers how app metrics can be implemented. 
Metrics are useful in helping you understand, in aggregate, how often an event occurred or how long an event took.

### Types of Metrics
In general, there are three types of metrics:

1. Counters - where you count the number of times an event happened
2. Timers - where you time duration of a code execution
3. Gauges - a value that can go up or down, e.g. temperature or memory usage

The ones we are primarily interested in are counters and timers.

An example of a counter is how many times an event occurs, e.g. order-checkout-request.

An example of a timer is how long it takes for a http request to execute.

### Dimensions
In addition, you can add dimensions to each metric (known as labels in other frameworks).
A dimension is an additional piece of information you can add to a metric.
Imagine attaching a map of key-value pairs to each metric.

An example could be a province, an event type, and so on.

### Common Misunderstandings
Metrics are meant to provide **aggregate** data, not to track individual results.

For example, if you had an online store, you wouldn't have a different metric for each different credit card transaction 
or the behavior of an individual customer. 
Instead, you would track metrics at a store level - how many transactions per hour, or how many users visited per day.

---

## Metrics in Spring Boot apps

Metrics are not stored within these apps. Using the actuator, spring boot apps can expose an endpoint for these metrics, 
which are scraped by various metrics frameworks. 
These metric returned is always the current value of the metric; historical data is unavailable.
It is the responsibility of the scraping tool, such as Prometheus, to keep the historical data in its own DB.

### Micrometer

The default instrumentation library for Spring Boot apps is Micrometer. It is included when you add the 
Spring Boot Actuator dependency to your project.

Gradle:
```groovy
dependencies {
    compile("org.springframework.boot:spring-boot-starter-actuator")
}
```

Maven:
```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
</dependencies>
```

Micrometer is like Slf4j, but for metrics - it provides a vendor-neutral facade to metrics.
You would need to include an implementation of the Micrometer interface, such as Datadog, New Relic, Prometheus, etc
so that the metrics is exported in a format that a vendor tool can understand.

For example, the Prometheus dependency is:

Gradle:
```groovy
compile 'io.micrometer:micrometer-registry-prometheus:latest.release'
``` 

Maven:
```xml
<dependency>
  <groupId>io.micrometer</groupId>
  <artifactId>micrometer-registry-prometheus</artifactId>
  <version>${micrometer.version}</version>
</dependency>
```

The full list of micrometer vendor implementations is available [here](https://micrometer.io/docs).

The configuration of the actuator for the different metrics vendors is available 
[here](https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-features.html#production-ready-metrics). 

Finally, you also need a tool to scrape the metrics, for example a Prometheus instance or the PCF Metrics Registrar.

### Naming Conventions

Since our Spring Boot apps use Micrometer, 
we need to use Micrometer's naming convention of lowercase words separated by the '.' (dot) character.
The vendor implementations will take care of transforming the lowercase-dot notation to that system's convention.

For example, if you had a metric `app.store.order.placed`, 
the Prometheus implementation would export it as `app_store_order_placed` automatically.

This is documented on the [Micrometer site](https://micrometer.io/docs/concepts#_naming_meters).

---

## Metrics on PCF Apps

When apps are deployed to PCF, your app can be configured to be scraped by the PCF Metrics Registrar.
By attaching the appropriate nozzle to the PCF Firehose, these metrics can flow to an indexing/monitoring tool, 
such as Splunk or DataDog.

### PCF Metrics

While PCF has a metrics page, known as PCF Metrics, the actually functionality is limited.
For example, it is difficult to change the time-scales, labels, titles, etc.

The PCF Metrics Registrar can parse metrics exported in the _prometheus_ format, exposed via `/actuator/prometheus`. 
To do this, we need to add the prometheus micrometer dependency in our apps. 
We also need to enable the prometheus actuator, but it should be enabled when the library is added.

If for some reason it is still disabled, you can enable it by adding the following to your application yml:

```yaml
management:
  metrics:
    export:
      prometheus:
        enabled: true
  endpoints:
    web:
      exposure:
        include: health,info,prometheus
```
__Note__: The health and info endpoints are also enabled above.

#### Limitations 
PCF Metrics currently **cannot parse dimensions**. This will hopefully be fixed at some point.
As a result, in a past project we ended up adding the extra metadata in the event name itself.

Alternatively if you bypass PCF metrics altogether and send metrics directly to metrics tool (like Splunk) itself, 
you may be able to add dimensions.

### Registering a metrics endpoint on PCF

For PCF Metrics to scrape the apps on PCF, we need to register the endpoints.
To do this, we'll have to install the metrics-registrar plugin to the cf cli.
We only have to do this one per app per space - however we need this for each space.
Unfortunately, we cannot do this via the manifest file as of yet.

```shell script
cf install-plugin -r CF-Community "metric-registrar"
```

e.g. Registering metrics for a 'store' app:
```shell script
cf register-metrics-endpoint store actuator/prometheus
```

Check which endpoints are registered:
```shell script
cf registered-metrics-endpoints
```

Unregister a metrics endpoint:
```shell script
cf unregister-metrics-endpoint [-p PATH]
```

---

## Additional Resources
- [Spring Boot Actuator](https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-features.html)
- [Spring Boot Actuator Metrics Configuration](https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-features.html#production-ready-metrics)
- [Micrometer Homepage](https://micrometer.io/)
- [Micrometer Naming Convention](https://micrometer.io/docs/concepts#_naming_meters)
- [PCF Metrics Registrar](https://docs.pivotal.io/pivotalcf/2-6/metric-registrar/using.html)
