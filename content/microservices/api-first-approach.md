---
title: "API-First Approach"
date: 2020-02-22T21:02:23-05:00
draft: true
---

---

In addition to proper API naming conventions, we want well-documented and usable APIs.
We can't assume our APIs will only be used by our developers who can always look at the code:
- separate teams may want to integrate with our APIs
- the service itself might be exposed to mobile/web apps in the future, who will need documentation
- non-technical testing teams might use APIs to test the functionality of the system.

## OpenAPI Specification

The [OpenAPI Specification](https://github.com/OAI/OpenAPI-Specification) (OAS) is a project which defines a standard, 
programming language-agnostic interface description for REST APIs.
The OpenAPI Specification 2.0 is based on and  is identical to the [Swagger](https://swagger.io) 2.0 specification.

## Springfox Swagger UI

[Springfox Swagger](https://springfox.github.io/springfox/docs/current/) is a Java project that implements the
Swagger 2.0 spec and generates machine and human-readable specs for JSON APIs. While it is not part of the Spring 
project, it integrates easily with Spring Boot apps.

The generated Swagger UI provides a user an API interface - where they can view the APIs, 
request and response body structures, as well as use the APIs on the running server. 
[Here](https://petstore.swagger.io/) is a sample Swagger UI from swagger.io. 
Springfox Swagger generates a page with similar capabilities.

### Installation and Configuration
We use typically use two Springfox libraries: `springfox-swagger2` and `springfox-swagger-ui`. 

To add the maven dependencies:
```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>

<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
```

To add the gradle dependencies:
```groovy
implementation group: 'io.springfox', name: 'springfox-swagger2', version: '2.9.2'
implementation group: 'io.springfox', name: 'springfox-swagger-ui', version: '2.9.2'
```

To enable the Swagger UI, you need to add the `@EnableSwagger2` annotation to a configuration class.
The configuration can also be customized. In the example below, only the APIs defined under the `@RestController` 
annotation are documented. By default, Springfox Swagger detects some error APIs from Spring, which this removes:

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
                .select()
                .apis(RequestHandlerSelectors.withClassAnnotation(RestController.class))
                .paths(PathSelectors.any())
                .build();
    }
}
``` 

To access it go to [http://localhost:8080/swagger-ui.html](http://localhost:8080/swagger-ui.html).
Update the host/port as needed.

You can also configure your app to redirect its base url to the Swagger UI page:
```java
@Controller
public class HomeController {
    @GetMapping(value = "/")
    public String index() {
        return "redirect:swagger-ui.html";
    }
}
```

## Additional Resources
- [API Naming Conventions]({{< relref "dev-practices/api-naming-conventions" >}})
- [Swagger Editor](https://swagger.io/tools/swagger-editor/)
- [Swagger Codegen](https://swagger.io/tools/swagger-codegen/)
