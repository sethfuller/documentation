# Definition
It is a Spring module that offers Rapid Application Development to
Spring framework. Spring module is used to create an application based
on Spring framework which requires to configure few Spring files.

# Advantages
Here are some major advantages of using spring-boot:

    - Helps you to create a stand-alone application, which can be started using java.jar.
    - It offers pinpointed‘started’ POMs to Maven configuration.
    - Allows you to Embed Undertow, Tomcat, or Jetty directly.
    - Helps you to configure spring whenever possible automatically.

# Spring Intializr
It is a web tool provided by Spring on its official website. However,
you can also create Spring Boot project by entering project details.

[Spring Initializr](https://start.spring.io/)

# Features

    - Starter dependency
    - Auto-configuration
    - Spring initializer

# Rapid Application Development (RAD)

This is a frequently asked job interview. Various phases of RAD mode are:

    - Business Modeling: Based on the flow of information and distribution between various business channels, the product is designed.
    - Data Modeling: The information collected from business modeling is refined into a set of data objects that are significant for the business.
    - Application Generation: Automated tools are used for the construction of the software, to convert process and data models into prototypes

## What is RAD
RAD or Rapid Application Development process is an adoption of the waterfall model; it targets developing software in a short period. RAD follow the iterative

SDLC RAD model has the following phases:

    - Business Modeling
    - Data Modeling
    - Process Modeling
    - Application Generation
    - Testing and Turnover

# Start Application
```
    java -jar project-name-0.0.1-SNAPSHOT.jar
```

# Configuration

In src/main/resources

    - application.properties
    - application.yml

# Spring Boot Starters
    
| Starter | Description |
| ------ | ----- |
| spring-boot-starter-parent | Enables Auto Config |
| **Web** | **Description** |
| spring-boot-starter-data-rest | Spring Data repositories over REST using Spring Data REST |
| spring-boot-starter-web |	It is used for building the web application, including RESTful applications using Spring MVC. It uses Tomcat as the default embedded container. |
| spring-boot-starter-web-services | Spring Web Services |
| **Test**| **Description** |
| spring-boot-starter-test | It is used to test Spring Boot applications with libraries, including JUnit, Hamcrest, and Mockito. |
| **Database** | **Description** |
| spring-boot-starter-data-jpa|JPA with Hibernate|
| spring-boot-starter-jdbc | It is used for JDBC with the Tomcat JDBC connection pool. |
| spring-boot-starter-data-elasticsearch|Elasticsearch search and analytics engine and Spring Data Elasticsearch |
| spring-boot-starter-data-mongodb| MongoDB document-oriented database and Spring Data MongoDB Reactive|
| **Monitoring** | **Description** |
|spring-boot-start-actuator||

# Spring Boot Actuator
[Actuator Endpoints](https://docs.spring.io/spring-boot/docs/current/reference/html/actuator.html)

# REST
[Spring Boot REST Examples](spring_boot_rest.md)
