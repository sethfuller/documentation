# Eureka

[Github Repo](https://github.com/Netflix/eureka)

Eureka sets up servers that know how to communicate with REST endpoints.
The REST endpoints are clients that subscribe to the Eureka server to locate other
REST endpoints.

Multiple Eureka servers can be created.

## Server
[Spring Documentation](https://cloud.spring.io/spring-cloud-netflix/reference/html/#spring-cloud-eureka-server)

#### pom.xml
```xml
    <dependencies>
        ...
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
		</dependency>
       ...
    </dependencies>
       ...
    </dependencies>
```

#### application.properties
```properties
spring.application.name=naming-server
server.port=8761

# This is the server so don't register as a client
eureka.client.register-with-eureka=false

# Do not fetch registry since the server creates the registry
eureka.client.fetch-registry=false

spring.config.import=optional:configserver:
```

## Client
[Spring Documentation](https://cloud.spring.io/spring-cloud-netflix/reference/html/#service-discovery-eureka-clients)

#### pom.xml

```xml
    <dependencies>
        ...
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
		</dependency>
       ...
    </dependencies>
```

#### application.properties

The Eureka client registers with the Eureka server using **server.application.name**.

```properties
spring.application.name=customer

eureka.client.serviceUrl.defaultZone=http://localhost:8761/eureka
```
