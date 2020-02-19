---
title: "Credential Management"
date: 2020-02-19T13:31:52-05:00
draft: true
---

---

Our microservices do not live in a vacuum, they integrate with our APIs and other systems. 
These systems will often require us to present some form of credentials.
This page describes some tools we can use to manage these credentials.

## Services from the PCF Marketplace

When binding to a service from the PCF marketplace, the credentials are provided to us via the `VCAP_SERVICES`
environment variable. We're not usually concerned about managing these credentials as they are provided to us by
the platform. The PCF Admin can enable a configuration to hide the credentials from being exposed to developers in the 
Platform Apps Manager. 

## CredHub

CredHub is a component designed for centralized credential management in Cloud Foundry. 
CredHub centralizes and secures credential generation, storage, lifecycle management, and access.

[CredHub Homepage](https://docs.cloudfoundry.org/credhub/)


### Usage
1. Create a CredHub Service Broker instance from the PCF Marketplace with a unique name. For example, `app-credhub`.
1. Bind the service instance to the applications that need it.
1. To set the value, go to the Settings page for the service instance. Under the Configure Instance section, click 
   "Add Parameter" and set the "Enter as JSON" flag to true. A textarea will appear where you can paste the JSON
   representation of your credentials. For example, you can set: 

    ```json
    {
      "app-name" : {
        "sendgrid" : {
          "token": "value1"
        }
      }
    }
    ```

1. The credentials will be accessible in your Spring application the same way you access environment variables. 
   You would be able to use the `@Value(${...})` annotation. The format for accessing the values would be of this form: 
   ```
   vcap.services.[credhub-service-broker-name].credentials.[credential-path]
   ```
   
   To access the token we declared above, you would use:
   ```
   vcap.services.app-credhub.credentials.app-name.sendgrid.token
   ```

## Additional Resources
- [Spring CredHub project](https://spring.io/projects/spring-credhub)
