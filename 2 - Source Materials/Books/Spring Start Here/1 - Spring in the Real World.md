2025-07-06 08:51

# 1 - Spring in the Real World
- Spring is an *application framework* that is part of Java ecosystem
- Application framework is a set of common software functionalities that provides foundation structure for developing an app
- It prevent the need to develop an application from scratch

## Why use frameworks
- We build app on top of frameworks
- Frameworks provides broad set of tools and functionalities
- Framework address functionalities that are common across applications
	- Logging
	- Transactions
	- Protection against vulnerabilities
	- Communications
	- Performance improvement mechanism (caching/data compression)
- The code to address this common functionalities could be much bigger than *business logic code*
- Business logic code implement the *business requirement/user expectation*
	- It is what make the application unique

## The Spring Ecosystem
- Spring Core
	- The fundamental, provide foundational capabilities like Spring context, Spring aspect, Spring expression language (SpEL)
	- Spring MVC - for developing web applications that serve HTTP requests
	- Spring Data Access - connect to SQL databases
	- Spring testing - for writing test

### Spring Core
- Based on the principle inversion of control (IoC)
- We give control to some other piece of software (Spring) rather than allowing the app to control the execution
- We instruct the framework on how to manage the code we write through configuration
- *Control* here means creating an instance or calling a method
- Aspect-oriented programming - intercept (apsecting) methods 

### Spring Data
- Expand the Data Access module of Spring to provide functionalities to connect to SQL and NoSQL databases

### Spring Boot
- Introduce the concept of "convention over configuration"
- Offers default configuration that can be customized
- Allow writing less code

## The Use of Spring
### Backend
- Executes on the server side
- Responsible for managing data and serving client applications' requests
- Spring IoC for managing object instance
- Spring MVC or Spring WebFlux for implementing the REST endpoints
- Spring Boot to ease complexity of configuration
- Spring Integration to work with Kafka
- Spring Data to connect to SQL/NoSQL database
- Spring Security to implement authentication and authorization

### Automation Test App
- An app to validate all flows of the tested system
- Spring IoC for managing object instance
- Spring Data for database access
- Spring MVC to simulate calls from other systems

### Other Application
- Spring can also used for desktop or mobile application

## When not to use frameworks
- Small footprint
	- E.g. server-less function
- Specific security requirements that restrict from using open-source framework
	- E.g.  defense or governmental organizations
- Too much customization effort required
	- Might be chosen the wrong framework
- Already have a functional app


# References
