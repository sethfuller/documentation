## Monitoring with Actuator

#### Maven Dependency
In the **pom.xml** file add the following starter dependency.

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

#### URLs
To see all of the Actuator endpoints that are activated type this URL:

```
    http://localhost:8080/actuator
```

### Endpoints

|Endpoint|Description|
|---|---|
|health|The health of the application. By default this is the only endpoint activated|
|beans|All of the beans defined in your application both by your developers and the Spring Framework|env|Show all variables set in the environment|
|metrics|Performance metrics for many aspects of your application|
|mappings|Request mappings details|

### Configuring Active Endpoints

In application.properties enter:
```
# Activate the listed endpoints
# management.endpoints.web.exposure.include=health, beans, metrics, mappings

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

#### Health

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
This is a partial sample of the values returned by the /env endpoint.

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

#### Metrics
Many metrics are exposed by Actuator.

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
