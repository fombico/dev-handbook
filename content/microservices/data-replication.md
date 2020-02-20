---
title: "Data Replication"
date: 2020-02-20T13:45:15-05:00
draft: true
---

---

## Introduction

In a microservices architecture, one of the principles is for the services to be loosely coupled. 
The best technology choices can be made for that microservice's needs. 
As such, the data store implementation is owned by that service and not shared across microservices. 

What happens if one service needs data from another service to perform its task?

Depending on the data required, the Data Replication pattern might be a good use case.
Data replication involves having a copy of the original data in another service.

In this page, we'll define the following terminology:
- _originating service_ - the service originally owning the data
- _replicating service_ - the service where the data is replicated 

## Implementation

The Data Replication pattern can be implemented in a pub-sub architecture. The _originating service_ publishes its
data to a queue, which the _replicating service_ subscribes to. The message payload would have all the information
required to replicate. Whenever the _originating service_ updates its data, 
it publishes a message to inform the _replicating services_.

## Use cases

This pattern might be suitable if the frequency of change is low, and if the amount of data to replicate is small.
A low rate of change minimizes the likelihood of data being out-of-sync.
A small amount of data reduces the number of fields we need to transmit and persist.
Additionally, if the impact of being processing the task with out-of-sync data is minimal, then the resilience and
lower latency may be worth it.  

## Advantages

- __Simpler Implementation__
    
    With the data already present, there is no need to mix database queries and API calls to the _originating service_
    when performing your task.
    
- __More resilience, less coupling, fewer points of failure__

    When the data is replicated, the _replicating service_ no longer requires the _originating service_ to be present.
    There is no need to handle failing API edge-cases.

- __Less APIs__

    As data replication occurs through a pub-sub, the _originating service_ does not need to expose additional APIs to
    fetch the data.

- __Lower Latency__

    With the data present in the database, no time is lost to API calls.
    
- __Avoid unnecessary I/O__

    REST APIs usually return a complete representation of the data, by first fetching all the fields for it.
    The data returned typically having more fields than you require.
    With Data replication, you can query only the fields you need.

## Disadvantages

- __Data may be out of sync__

    When the _originating service_ updates its data, there will be a period of time when the _replicating services_ are
    out of sync. An investigation should be done to determine what the impact would be if the task was processed with
    out-of-sync data. If it is high, it might be worth investigating how to monitor the data replication progress.
    Data replication is [eventually-consistent](https://en.wikipedia.org/wiki/Eventual_consistency).

- __Potentially new technology may be required__

    The implementation uses a messaging framework, such as RabbitMQ or Kafka. If the team is unfamiliar with these
    frameworks, a ramp-up would be required. 

- __Additional setup required__

    If the team needs to introduce a messaging framework solely for this purpose, it will result in additional 
    project setup, extra deployment steps, and cost to run a messaging framework instance.
    
- __Overhead of replicated data__

    The replicated data needs to be persisted in a data store. This contributes to the cost to maintain the project. 
    If the _replicating service_ previously did not require a data store, it will need one now.

- __Increased Privacy Concerns__

    If the data to be replicated is sensitive, this will increase the attack surface of the system. 
    Investigate whether this pattern is still suitable.
    
## Additional resources

- [Eventual Consistency](https://en.wikipedia.org/wiki/Eventual_consistency)