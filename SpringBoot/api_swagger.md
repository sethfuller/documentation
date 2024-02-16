# Swagger

## Dependency
### Spring Boot 3
#### Maven

```xml
    <dependencies>
        ...
		<dependency>
			<groupId>org.springdoc</groupId>
			<artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
			<version>2.0.3</version>
		</dependency>
       ...
    </dependencies>
```

#### Gradle
```groovy
dependencies {
    implementation group: 'org.springdoc', name: 'springdoc-openapi-starter-webmvc-ui', version: '2.0.3'
}
```

### Spring Boot 2
#### Maven
```xml
    <dependencies>
        ...
		<dependency>
			<groupId>org.springdoc</groupId>
			<artifactId>springdoc-openapi-ui</artifactId>
			<version>1.6.15</version>
		</dependency>
       ...
    </dependencies>
```

#### Gradle
```groovy
dependencies {
    implementation group: 'org.springdoc', name: 'springdoc-openapi-ui', version: '1.6.15'
}
```

## URLs
```
    http://localhost:8080/swagger-ui/index.html
```

```
    http://localhost:8080/v3/api-docs
```

## Documentation
You can add tags to document the API in the generated Swagger documentation.

```java
import io.swagger.v3.oas.annotations.tags.Tag;

@Tag(name = "Customer", description = "Customer management APIs")
@RestController
public class CustomerController {
}
```
