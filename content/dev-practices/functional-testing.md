---
title: "TDD, Functional Testing"
date: 2020-02-16T23:28:48-05:00
draft: true
weight: 20
---

---

This section describes approaches to TDD and functional testing.

- [TDD Schools of Thought](#tdd-schools-of-thought)
- [Test Doubles](#test-doubles)
- [Common Testing Patterns](#common-testing-patterns)
- [Testing Pyramid](#testing-pyramid)

{{% notice info %}}
Consider expanding each list item into separate pages.
{{% /notice %}}

## TDD Schools of Thought

### Inside-Out 
- Also known as "Classical School", "Bottom-up", "Detroit" or "Chicago" school of TDD
- Begin at an inner component and work your way out
- Leads to more design patterns
- `AssertEquals` - state verification

### Outside-In
- Also known as "London School", "Top-down" approach
- Begin at an outer component and work your way in
- Typical starting point is Integration test or at an entry-point (e.g. controller)
- Leads to YAGNI - You Ain’t Gonna Need It
- `verify()` - behavior verification

---

## Test Doubles

### Dummy
- Objects passed around but never used
- E.g. `any(String.class)` as a parameter in a verify assertion

### Fakes
- Objects with working implementations, but not suitable for production
- E.g. H2 Database instead of MySQL

### Stubs
- Objects that ‘stub out’ an implementation, usually with interfaces
- Can be used to verify behavior or state

### Mocks
- Used for verifying behavior, by first defining the expectations before the tested code is executed
- E.g. in Java:
    ```java 
    when(ovenService.bakePizza(ingredients)).thenReturn(bakedPizza);
    
    subject.get(ingredients);
    
    verify(ovenService).bakePizza(ingredients);
    ```

### Spies
- For verifying behavior, similar to a mock, but without pre-defining the expectations before the tested code is executed
- E.g. Counting how many times the object’s method was called

---

## Common Testing Patterns

### Testing Static behavior

A common suggestion for Java projects we've heard is to use PowerMock - for mocking static, private, or final methods, and external libraries

Our team __does not use or advocate for__ using PowerMock:
- For static methods, we’d typically avoid making them in the first place as they make testing challenging. 
  In addition, since static methods can only easily call other static methods, when you refactor out common code, 
  you end up breeding more static methods, making code progressively harder to test
- Testing private methods that we wrote/own is not something you would do when following 
  TDD practices - the common code in a public method would get refactored out into private methods, 
  which should be covered by the existing tests.
- External libraries can be wrapped, which would also provide us with a common interface to external API, effectively. 
  This is useful for logging or debugging, or in case we want to change libraries.

### External Libraries

This falls under the discussion of PowerMock above. The best approach is to wrap it in an interface/class.

### Time
- A typical use case is a timestamp, which usually uses a static method
- One option is to wrap the timestamp generation in a wrapper class, which would allow you to mock the response, 
  while at the same time consolidating the date format into a central class
- Alternatively, when doing equality assertions, you can assert while ignoring timestamp-related fields

### DB Cleanup
Occasionally some tests use an in-memory DB - such as in integration tests. 
However, the database tables are not cleaned up between tests by default, which may lead to inconsistent behavior. 
As such, a common practice is to clean up the database before each test, in the @BeforeEach (JUnit5) code block

---

## Testing Pyramid

We have three layers to our pyramid:
- Unit tests 
    - tests around a single class
    - tend to be more numerous, cover more edge cases, and are the fastest to run
- Integration tests 
    - tests multiple classes, and/or integrating with a framework
    - e.g. testing a controller, database, or REST client
    - tend to be fewer in number, cover less edge cases, and take longer to run than unit tests
- UI/Acceptance test
    - tests from front-end to back-end, or across multiple services
    - tend to be the fewest in number, usually covering only happy paths, and take the longest to run

<!-- Add the query param width=100%, otherwise header links break -->
![Testing Pyramid](/images/dev-practices/testing-pyramid.svg?width=100%)
