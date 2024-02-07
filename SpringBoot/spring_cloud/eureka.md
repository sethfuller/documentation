# Eureka

[Github Repo](https://github.com/Netflix/eureka)

## Server
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

## Client
[Spring Documentation](https://cloud.spring.io/spring-cloud-netflix/reference/html/#service-discovery-eureka-clients)

#### pom.xml

```xml
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
		</dependency>
```

#### application.properties

```properties
eureka.client.serviceUrl.defaultZone=http://localhost:8761/eureka
```
