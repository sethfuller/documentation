## Monitoring with Actuator
The Actuator module provides all of Spring Boot's production-ready features.

### Dependency
#### Maven
In the **pom.xml** file add the following starter dependency to activate Actuator.

```xml
    <dependencies>
        ...
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>
       ...
    </dependencies>
```
#### Gradle

```groovy
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-actuator'
}
```

#### URL
To see all of the Actuator endpoints that are activated type this URL:

```
    http://localhost:8080/actuator
```

### Endpoints
This is a partial listing of the Actuator endpoints. For a complete list of Actuator endpoints
see [Actuator Documentation](https://docs.spring.io/spring-boot/docs/current/reference/html/actuator.html)

Sanitization in the descriptions below means that unless configured to show values all the
values will be displayed as asterisks.

|Endpoint|Description|
|---|---|
|health|The health of the application. By default this is the only endpoint activated|
|beans|All of the Spring beans defined in your application both by your developers and the Spring Framework|env|Exposes properties from Spring’s ConfigurableEnvironment. Subject to **sanitization**.|
|configprops|Displays a collated list of all @ConfigurationProperties. Subject to **sanitization**.|
|metrics|Metrics for many aspects of your application|
|mappings|Displays a collated list of all @RequestMapping paths.|

### Configuring Active Endpoints

In application.properties enter:
```
# Activate the listed endpoints
# management.endpoints.web.exposure.include=health,beans,metrics,mappings

# Activate all actuator endpoints
management.endpoints.web.exposure.include=*

# Show the values set for configuration values
# The default is NEVER
# The other valid value is WHEN_AUTHORIZED
management.endpoint.configprops.show-values=ALWAYS

# Show all variables set for the application by configuration, or in the system
# Valid values: ALWAYS, NEVER (default), WHEN_AUTHORIZED
management.endpoint.env.show-values=ALWAYS
```

### Security
If your application is exposed to the public Actuator endpoints should be secured. If
Spring Security is on the classpath and no other SecurityFilterChain bean is present, all
actuators other than /health are secured by Spring Boot auto-configuration. If you define a custom
SecurityFilterChain bean, Spring Boot will use that to secure the endpoints instead.

This example of a SecurityFilterChain bean is from the Spring Boot Actuator documentation.
It only allows access to Actuator endpoints if the user has **ENDPOINT_ADMIN** permission.

```java
import org.springframework.boot.actuate.autoconfigure.security.servlet.EndpointRequest;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.web.SecurityFilterChain;

import static org.springframework.security.config.Customizer.withDefaults;

@Configuration(proxyBeanMethods = false)
public class MySecurityConfiguration {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http.securityMatcher(EndpointRequest.toAnyEndpoint());
        http.authorizeHttpRequests((requests) -> requests.anyRequest().hasRole("ENDPOINT_ADMIN"));
        http.httpBasic(withDefaults());
        return http.build();
    }

}

```

To allow unauthenticated access to all of the Actuator endpoints replace the line:
```java
        http.authorizeHttpRequests((requests) -> requests.anyRequest().hasRole("ENDPOINT_ADMIN"));
```

With:
```java
        http.authorizeHttpRequests((requests) -> requests.anyRequest().permitAll());
```

#### Health

##### URL

```
    http://localhost:8080/actuator/health
```

##### Response
```json
{
    "status": "UP"
}
```

#### Beans

##### URL

```
    http://localhost:8080/actuator/beans
```

##### Response
```json
{
  "contexts": {
    "application": {
      "beans": {
        "endpointCachingOperationInvokerAdvisor": {
          "aliases": [],
          "scope": "singleton",
          "type": "org.springframework.boot.actuate.endpoint.invoker.cache.CachingOperationInvokerAdvisor",
          "resource": "class path resource [org/springframework/boot/actuate/autoconfigure/endpoint/EndpointAutoConfiguration.class]",
          "dependencies": [
            "org.springframework.boot.actuate.autoconfigure.endpoint.EndpointAutoConfiguration",
            "environment"
          ]
        },
        ...
        "studentController": {
          "aliases": [],
          "scope": "singleton",
          "type": "net.javaguides.springbootrestapi.controller.StudentController",
          "resource": "file [/Users/sethfuller/Src/Languages/Java/Spring/SpringBoot/springboot-rest-api/target/classes/net/javaguides/springbootrestapi/controller/StudentController.class]",
          "dependencies": []
        },
        ...
      }
    }
  }
}
```

#### Environment
The /actuator/env endpoint returns all configured values for the application and the system
it is running on.

##### URL

```
    http://localhost:8080/actuator/env
```
##### Configuration
The application.properties must be configured with:
```
# Show all variables set for the application by configuration, or in the system
# Valid values: ALWAYS, NEVER (default), WHEN_AUTHORIZED
management.endpoint.env.show-values=ALWAYS
```

##### Response
This is a partial sample of the values returned by the /actuator/env endpoint.

```json
{
  "activeProfiles": [],
  "propertySources": [
    {
      "name": "server.ports",
      "properties": {
        "local.server.port": {
          "value": 8080
        }
      }
    },
    {
      "name": "servletContextInitParams",
      "properties": {}
    },
    ...
    {
      "name": "systemEnvironment",
      "properties": {
        "JAVA_HOME": {
          "value": "/opt/homebrew/Cellar/openjdk/21.0.2/libexec/openjdk.jdk/Contents/Home",
          "origin": "System Environment Property \"JAVA_HOME\""
        },
        ...
      }
    },
    {
      "name": "Config resource 'class path resource [application.properties]' via location 'optional:classpath:/'",
      "properties": {
        "management.endpoints.web.exposure.include": {
          "value": "*",
          "origin": "class path resource [application.properties] - 1:43"
        },
        "management.endpoint.configprops.show-values": {
          "value": "ALWAYS",
          "origin": "class path resource [application.properties] - 3:45"
        },
        "management.endpoint.env.show-values": {
          "value": "ALWAYS",
          "origin": "class path resource [application.properties] - 4:37"
        }
      }
    },
    ...
   ]
```

#### Environment
The /actuator/env endpoint returns all configured values for the application and the system
it is running on.

##### URL

```
    http://localhost:8080/actuator/env
```
##### Configuration
The application.properties must be configured with:
```
# Show all variables set for the application by configuration, or in the system
# Valid values: ALWAYS, NEVER (default), WHEN_AUTHORIZED
management.endpoint.configprops.show-values=ALWAYS
```

##### Response
This is a partial sample of the values returned by the /actuator/configprops endpoint.

```json
{
  "contexts": {
    "application": {
      "beans": {
        "spring.ssl-org.springframework.boot.autoconfigure.ssl.SslProperties": {
          "prefix": "spring.ssl",
          "properties": {
            "bundle": {
              "pem": {},
              "jks": {},
              "watch": {
                "file": {
                  "quietPeriod": "PT10S"
                }
              }
            }
          },
          "inputs": {
            "bundle": {
              "pem": {},
              "jks": {},
              "watch": {
                "file": {
                  "quietPeriod": {}
                }
              }
            }
          }
        },
        ...
        "management.endpoint.configprops-org.springframework.boot.actuate.autoconfigure.context.properties.ConfigurationPropertiesReportEndpointProperties": {
          "prefix": "management.endpoint.configprops",
          "properties": {
            "showValues": "ALWAYS",
            "roles": []
          },
          "inputs": {
            "showValues": {
              "value": "ALWAYS",
              "origin": "class path resource [application.properties] - 3:45"
            },
            "roles": []
          }
        },
        ...
      }
    }
  }
}
```

#### Metrics
Many metrics are exposed by Actuator.

##### URL

```
    http://localhost:8080/actuator/metrics
```

##### Response
This is a partial listing of the response from the /actuator/metrics endpoint.

```json
{
  "names": [
    "application.ready.time",
    "application.started.time",
    "disk.free",
    "disk.total",
    "executor.active",
    "executor.completed",
    "executor.pool.core",
    "executor.pool.max",
    "executor.pool.size",
    "executor.queue.remaining",
    "executor.queued",
    "http.server.requests",
    "http.server.requests.active",
    "jvm.buffer.count",
    "jvm.buffer.memory.used",
    "jvm.buffer.total.capacity",
    "jvm.classes.loaded",
    ...
   ]
}
```

You can add any of the metrics to the end of the /actuator/metrics endpoint to get the specific
metrics information.

##### URL

```
    http://localhost:8080/actuator/metrics/executor.active
```

##### Response

```json
{
  "name": "executor.active",
  "description": "The approximate number of threads that are actively executing tasks",
  "baseUnit": "threads",
  "measurements": [
    {
      "statistic": "VALUE",
      "value": 0.0
    }
  ],
  "availableTags": [
    {
      "tag": "name",
      "values": [
        "applicationTaskExecutor"
      ]
    }
  ]
}
```

#### Mappings
This endpoint will show you all of the endpoints mapped for your application, both those
defined by the application and those defined by the Spring Framework.

##### URL

```
    http://localhost:8080/actuator/mappings
```

##### Response
This is a portion of the /acuator/mappings endpoint response. It describes the /students/{id}
endpoint. The mappings endpoint also describes all of the endpoints defined by the Spring
Framework, such as each enabled actuator endpoint.

```json
{
  "contexts": {
    "application": {
      "mappings": {
        "dispatcherServlets": {
          "dispatcherServlet": [
              ...
            {
              "handler": "net.javaguides.springbootrestapi.controller.StudentController#getStudent(int)",
              "predicate": "{GET [/students/{id}]}",
              "details": {
                "handlerMethod": {
                  "className": "net.javaguides.springbootrestapi.controller.StudentController",
                  "name": "getStudent",
                  "descriptor": "(I)Lorg/springframework/http/ResponseEntity;"
                },
                "requestMappingConditions": {
                  "consumes": [],
                  "headers": [],
                  "methods": [
                    "GET"
                  ],
                  "params": [],
                  "patterns": [
                    "/students/{id}"
                  ],
                  "produces": []
                }
              }
            },
       ...
    ]
```
