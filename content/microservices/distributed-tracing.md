---
title: "Distributed Tracing"
date: 2020-02-19T21:14:01-05:00
draft: true
---


## Introduction

In a microservices architecture, incoming requests and responses are processed across multiple systems. 
Some requests are synchronous, via HTTPS, or perhaps via RPC.
Other requests are asynchronous, using a messaging framework like RabbitMQ or Apache Kafka.
Each of these microservices may encounter an unhandled exception case or throw a validation error.
As such, we'll want a way to trace a request across our system to debug an issue.


## Spring Cloud Sleuth

Spring Cloud Sleuth is a distributed tracing solution for the Spring Cloud Framework.

Spring Cloud Sleuth introduces the concept of traces and spans as part of tracing.
A __span__ is a basic unit of work. Receiving and processing a request are part of a span. Making another api call as
part of handling a request will generate a new span in the receiving service. The tree of spans make up a __trace__.
By adding the `spring-cloud-starter-sleuth` dependency to the classpath for each of your services, 
Spring Sleuth will automatically create traces and spans and assign ids to them.

See this [link](https://cloud.spring.io/spring-cloud-sleuth/reference/html/#sleuth-adding-project) for adding 
Spring Cloud Sleuth to your project. 

When your service initially makes a request, a trace id is created and remains constant for that request.
A span id is also created for each hop for that request. The first span id is the same as trace id. 
These trace and span ids appear in the logs of your app. 

Say we have microservices for a pizzeria franchise. We have an Order service where you can POST a new order.
Upon doing so, an async event is sent via RabbitMQ to an Oven service.
When the Oven service receives the event, it makes a GET call to the Order service to fetch additional details.

Order service's logs may look like:
```plaintext
2020-02-19 22:16:15.300  INFO [Order,ecd89a9e6aafbf93,ecd89a9e6aafbf93,true]  28021 --- [nio-8080-exec-4] com.fombico.order.ApiController           : POST: created new Order 1000
2020-02-19 22:16:15.347  INFO [Order,ecd89a9e6aafbf93,bb3be78698f232e3,true]  28021 --- [nio-8080-exec-5] com.fombico.order.ApiController           : GET: Retrieving order details for order 1000 
```

Oven service's logs look like:
```plaintext
2020-02-19 22:16:15.327  INFO [Oven,ecd89a9e6aafbf93,9a9c7bd4b5d3e8e8,true]  28080 --- [-my_biE84tUrQ-1] com.fombico.oven.MessagingService        : Received order message: Order 1000
2020-02-19 22:16:15.351  INFO [Oven,ecd89a9e6aafbf93,9a9c7bd4b5d3e8e8,true]  28080 --- [-my_biE84tUrQ-1] com.fombico.oven.MessagingService        : Fetched order details: Pepperoni Pizza
```

In this example the trace id is `ecd89a9e6aafbf93`, consisting of 3 spans: `ecd89a9e6aafbf93`, `9a9c7bd4b5d3e8e8`, and `bb3be78698f232e3`.
- The span `9a9c7bd4b5d3e8e8` is opened when Oven service receives the message, 
  and is only closed when it receives the response from Order service.
- The span `bb3be78698f232e3` is opened and closed before `9a9c7bd4b5d3e8e8` is closed.
- The trace and span is automatically created for both a HTTP requests and passed through to RabbitMQ messages

### Adding extra attributes

While traces and spans are useful, we may want to add more context to debug our requests across the system. 
For example, say each pizzeria had a store id.

Adding the extra attributes across our system involves two things:
- showing them in logs
- passing them to other services

To show the extra attributes in the logs, we need to override logging pattern level in the application.yml. 
Here we set the prefix with the extra attribute at the end (`[%X{store-id:-}]`):
```yaml
logging:
  level:
    pattern.level: "%5p [${spring.zipkin.service.name:${spring.application.name:-}},%X{X-B3-TraceId:-},%X{X-B3-SpanId:-},%X{X-Span-Export:-}][%X{store-id:-}] "
```

We also need to configure Sleuth properties to add the attribute to a whitelist and add it as a baggage key to 
propagate it to other services. This would look like:
```yaml
sleuth:
  sampler:
    probability: 1.0
  baggage-keys:
    - "store-id"
  log.slf4j.whitelisted-mdc-keys:
    - "store-id"
```

We set the values of the extra baggage keys using the Sleuth `ExtraFieldPropagation` class. 
We set the values for the logs using the `MDC` class. Altogether, the code in Java may look like:

```java
private void configureSleuthAndLogs(String storeId) {
    ExtraFieldPropagation.set("store-id", storeId);
    MDC.put("store-id", storeId);
}
```

The Java code only needs to be set in the starting service. 
__The logging pattern and sleuth properties needs to be set for all services.__ 

With these extra fields, the Order service's logs could look like:
```plaintext
2020-02-19 22:26:32.557  INFO [Order,8a1950c642121725,8a1950c642121725,true][136547]  28021 --- [io-8080-exec-10] com.fombico.order.ApiController           : POST: created new Order 1000
2020-02-19 22:26:32.661  INFO [Order,8a1950c642121725,acb11fad4d0d490b,true][136547]  28021 --- [nio-8080-exec-2] com.fombico.order.ApiController           : GET: Retrieving order details for order 1000
```

While Oven service's logs could look like:
```plaintext
2020-02-19 22:26:32.598  INFO [Oven,8a1950c642121725,eae37de2e6bfead4,true][136547]  28080 --- [-my_biE84tUrQ-1] com.fombico.oven.MessagingService        : Received order message: Order 1000
2020-02-19 22:26:32.669  INFO [Oven,8a1950c642121725,eae37de2e6bfead4,true][136547]  28080 --- [-my_biE84tUrQ-1] com.fombico.oven.MessagingService        : Fetched order details: Pepperoni Pizza
```

You can see the store-id is set to "136547".

For reference, when Sleuth is added, it overrides the default log pattern with:
```plaintext
 %5p [${spring.zipkin.service.name:${spring.application.name:-}},%X{X-B3-TraceId:-},%X{X-B3-SpanId:-},%X{X-Span-Export:-}]
```

## Additional Resources

- [Spring Cloud Sleuth](https://spring.io/projects/spring-cloud-sleuth) 
