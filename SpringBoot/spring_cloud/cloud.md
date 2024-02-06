# Spring Cloud Libraries

## Netflix Eureka
[Github Repo](https://github.com/Netflix/eureka)

### Server
[Spring Documentation](https://cloud.spring.io/spring-cloud-netflix/reference/html/#spring-cloud-eureka-server)
#### pom.xml
```xml
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
		</dependency>
```

#### application.properties
```properties
spring.application.name=naming-server
server.port=8761

eureka.client.register-with-eureka=false
eureka.client.fetch-registry=false

spring.config.import=optional:configserver:
```

### Client
[Spring Documentation](https://cloud.spring.io/spring-cloud-netflix/reference/html/#service-discovery-eureka-clients)
#### pom.xml
```xml
```

#### application.properties
```properties
```

## Feign
[Feign Info](./feign.md)


## Spring Cloud Gateway
[Spring Documentation](https://spring.io/projects/spring-cloud-gateway)

#### pom.xml
```xml
```

### application.properties
```properties
```

## Resiliance 4J

[Documentation](https://resilience4j.readme.io/docs/getting-started)

#### pom.xml
```xml
```

#### application.properties
```properties
```

