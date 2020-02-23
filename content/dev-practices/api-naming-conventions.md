---
title: "API Naming Conventions"
date: 2020-02-17T10:30:33-05:00
draft: true
weight: 50
---

---

This page describes general naming conventions for APIs.

## General Guidelines
- Use hyphens
- Do not use underscores
- Use lowercase letters
- Version your APIs

## Examples

| Base URL Pattern | Example URL | Purpose |
|------------------|-------------|---------|
| /v1/{aggregate-plural-form}/{id} | /v1/exceptions/101 | Base URL for an entity |
| /v1/{parent-aggregate-plural-form}/{parent-id}/{child-aggregate-plural-form}/{child-id} | /v1/stores/101/orders/202 | Base URL for parent/child |

&nbsp;

| Verb | URL Pattern | Example URL | Purpose |
|------|-------------|-------------|---------|
| GET | /v1/{aggregate-plural-form}/{aggregate-id} | /v1/exceptions/101 | Get a single entity |
| POST | /v1/{aggregate-plural-form} | /v1/exceptions | Create a single entity | 
| DELETE | /v1/{aggregate-plural-form}/{aggregate-id} | /v1/exceptions/101 | Delete a single entity |
| PUT | /v1/{aggregate-plural-form}/{aggregate-id} | /v1/exceptions/101 | Update all attributes of single entity |
| POST | /v1/{aggregate-plural-form}/{aggregate-id}/actions | /v1/exceptions/101/actions | Perform one or more actions for a single entity |
| POST | /v1/{aggregate-plural-form}/{aggregate-id}/actions/{name} | /v1/exceptions/101/actions/acknowledge | Perform a named action on single entity |
| GET | /v1/{aggregate-plural-form} | /v1/exceptions | Get multiple entities of a kind |
| POST | /v1/{aggregate-plural-form}/actions | /v1/exceptions/actions | Perform one or more actions for multiple entities |
| POST | /v1/{aggregate-plural-form}/actions/{name} | /v1/exceptions/actions/acknowledge | Perform a named action on multiple entities |

## Additional Resources
- [API-First Approach]({{< relref "microservices/api-first-approach" >}})

