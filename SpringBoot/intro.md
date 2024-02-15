
# Spring Boot, REST, Database, and Spring Cloud

# Spring Framework

# Spring Boot

## Starters

### Parent
```xml
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>3.2.2</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
```

### Web
[Web Features](spring_boot_web.md)

### Data JPA
```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>
```

### Databases

#### H2
This is an in memory database good for development and testing. When the application is
restarted the database is reinitialized and any changes are removed.

```xml
		<dependency>
			<groupId>com.h2database</groupId>
			<artifactId>h2</artifactId>
            <scope>runtime</scope>
		</dependency>
```

#### MySql
##### Spring Boot 3.0.x and Lower
```xml
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-jaja</artifactId>
			<scope>runtime</scope>
		</dependency>
```

##### Spring Boot 3.1 and Greater
```xml
		<dependency>
			<groupId>com.mysql</groupId>
			<artifactId>mysql-connector-j</artifactId>
			<scope>runtime</scope>
		</dependency>
```
### Cloud

### Helpers
#### Lombok
```xml
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<optional>true</optional>
		</dependency>
```

### Diagnostics
#### Actuator
```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>

```

### Documentation
#### HATEOAS
```xml
```xml
		<dependency>
			<groupId>org.springframework.data</groupId>
			<artifactId>spring-data-rest-hal-explorer</artifactId>
		</dependency>
```

### Dev Tools
```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
			<optional>true</optional>
		</dependency>
```

### Testing
```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
```
