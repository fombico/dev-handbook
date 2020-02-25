---
title: "Twelve-Factor App"
date: 2020-02-24T21:18:26-05:00
draft: true
---

---
[The Twelve-Factor App](https://12factor.net/) introduces a methodology for building micro-services.
If you haven't already done so, read through the twelve factors site.
This page aims to provide supplementary information about each of the factors, from experiences on past projects.

## I. Codebase
__One codebase tracked in revision control, many deploys__

## II. Dependencies
__Explicitly declare and isolate dependencies__

This factor promotes the use of the standard build tool for declaring dependencies to your project.
In a typical Java project, you would be able to meet this easily by using the Maven or Gradle build tool.

One other benefit of explicitly declaring dependencies is to simplify setup for new developers,
so that they would have everything they need to run the app. However, when it comes to microservices, 
we often need other services running locally to mimic the deployment environment.
This is one of the twelve factors - [X. Dev/prod parity](#x-devprod-parity).
For example, RabbitMQ/Kafka, Redis, or perhaps a MongoDB instance.

Now if we also had a `docker-compose.yml` with our backing services declared:
```yaml
version: '3'
services:
  
  rabbitmq:
    image: rabbitmq:3-management
    ports:
      - "5672:5672"
      - "15672:15672"

  redis:
    image: redis:alpine
    ports:
      - "6379:6379"
```

We'd be able to start up all of our local services with a single command:
```shell script
docker-compose up -d

# to stop the services
docker-compose stop
```

## III. Config
__Store config in the environment__

## IV. Backing services
__Treat backing services as attached resources__

## V. Build, release, run
__Strictly separate build and run stages__

## VI. Processes
__Execute the app as one or more stateless processes__

## VII. Port binding
__Export services via port binding__

This is not usually an issue in Spring Boot apps. They come with an HTTP Server (Tomcat/Netty) and don't require being
bound to a different HTTP server. In the cloud they're deployed on separate containers and don't share ports with other
services.

When running locally, the backing services that your apps talk to (RabbitMQ, MySQL, Redis, etc) are also usually
auto-configured in Spring and don't share the same ports. It's when your app talks to other locally running web servers
that you adjust their `server.port` values.

When running on the cloud, your app can easily connect to backing services by using the corresponding
[Spring Cloud Connector](https://cloud.spring.io/spring-cloud-connectors/).

## VIII. Concurrency
__Scale out via the process model__

## IX. Disposability
__Maximize robustness with fast startup and graceful shutdown__

## X. Dev/prod parity
__Keep development, staging, and production as similar as possible__

## XI. Logs
__Treat logs as event streams__

## XII. Admin processes
__Run admin/management tasks as one-off processes__