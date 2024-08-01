# Spring Boot REST
[Spring Boot Main Page](spring_boot.md)
[Rest Annotations Tutorial](https://spring.io/guides/tutorials/rest)
[Spring REST Annotations Java67](https://www.java67.com/2019/04/top-10-spring-mvc-and-rest-annotations-examples-java.html)

## Application
|Annotation|Description|
|----|----|
|@SpringBootApplication|The main class for the Spring Boot Application|

```java
package com.sethfuller;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class UserServiceApplication {

	public static void main(String[] args) {
		SpringApplication.run(UserServiceApplication.class, args);
	}
}
```

## RestController
|Annotation|Description|
|----|----|
|@RestController|Includes @Controller and @ResponseBody|
|@RequestMapping("v1")|For this class all requests must be preceeded by "v1" in the path|
|@ResponseStatus(HttpStatus.CREATED)|When the POST succeeds return the Created status code|
|@GetMapping("/users")|On GET Request Get the Users|
|@GetMapping("/users/{id}")|On GET Request Get the User for the ID|
|@PostMapping("/users"|On POST request create an entity with the Request Body|
|@DeleteMapping("/users/{id}")|When a DELETE request is recieved with an id field process with this method|
|@PathVariable("id") String userId|Extract the variable with name "id" and place it in userId|
|@ResponseBody|This request returns data in the body|

```java
package com.sethfuller.controller;

import java.util.List;
import java.util.Optional;

import javax.validation.Valid;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PatchMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseStatus;
import org.springframework.web.bind.annotation.RestController;
import com.sethfuller.model.PatchUserRequest;
import com.sethfuller.model.User;
import com.sethfuller.service.UserService;

    @RestController
    @RequestMapping("v1")
    public class UserController {
        @Autowired
        private UserService service;

        @GetMapping("/users/{id}")
        @ResponseBody
        public User getUser(@PathVariable("id") String id) {
            User user = service.getUserById(id);
            return user;
        }

        @GetMapping("/users")
	    public List<User> getUsers() {
		    return service.getUsers();
	    }

        @PostMapping("/users")
	    @ResponseStatus(HttpStatus.CREATED)
	    public void create(@Valid @RequestBody User user) {
		    service.create(user);
	    }
        
	    @DeleteMapping("/{id}")
	    public void delete(@PathVariable("id") String userId) {
		    var user = Optional.ofNullable(userId)
				.map(u -> Long.valueOf(userId))
				.map(service::getUser)
				.orElseThrow();
					
            service.delete(user.getUserId());
	    }
	
    }
```

# Exception Handling for Controller
|Annotation|Description|
|----|----|
|@ControllerAdvice|This class is used to handle exceptions for all RestController classes |
|@ExceptionHandler(MethodArgumentNotValidException.class)|This method handle the specified exception class|

```java
package com.sethfuller.exceptions;

import java.util.NoSuchElementException;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.http.converter.HttpMessageNotReadableException;
import org.springframework.web.HttpRequestMethodNotSupportedException;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;

import com.sethfuller.model.ErrorMessage;

@ControllerAdvice
public class BaseExceptionHandler {

	@ExceptionHandler(MethodArgumentNotValidException.class)
	public ResponseEntity<ErrorMessage> handleNotValidExeption(MethodArgumentNotValidException e) {
		var errors = e.getAllErrors();

		ErrorMessage message = null;

		if (errors != null && !errors.isEmpty()) {
			message = new ErrorMessage(400, errors.get(0).getDefaultMessage());
			return new ResponseEntity<ErrorMessage>(message, HttpStatus.BAD_REQUEST);
		}

		message = new ErrorMessage(400, "Bad Request");
		return new ResponseEntity<ErrorMessage>(message, HttpStatus.BAD_REQUEST);
	}

	@ExceptionHandler({ NoSuchElementException.class, NumberFormatException.class })
	public ResponseEntity<ErrorMessage> handleNotFoundException(Exception e) {
		ErrorMessage message = new ErrorMessage(404, "Not Found");
		return new ResponseEntity<ErrorMessage>(message, HttpStatus.NOT_FOUND);
	}

	@ExceptionHandler(IllegalArgumentException.class)
	public ResponseEntity<ErrorMessage> handleIllegalException(IllegalArgumentException e) {
		ErrorMessage message = new ErrorMessage(400, e.getMessage());
		return new ResponseEntity<ErrorMessage>(message, HttpStatus.BAD_REQUEST);
	}

	@ExceptionHandler({ HttpMessageNotReadableException.class, HttpRequestMethodNotSupportedException.class })
	public ResponseEntity<ErrorMessage> handleNotReadableException(Exception e) {
		ErrorMessage message = new ErrorMessage(400, "Bad Request");
		return new ResponseEntity<ErrorMessage>(message, HttpStatus.BAD_REQUEST);
	}
}

```

## Service
|Annotation|Description|
|----|----|
|@Service|This is a Service class|
|@Transactional|Updates/Inserts/Deletes are transactional|

```java
package com.sethfuller.service;

import java.util.List;

import com.sethfuller.model.PatchUserRequest;
import com.sethfuller.model.User;

public interface UserService {
	public List<User> getUsers();
	public User getUser(Long userId);
	public void create(User user);
	public void delete(Long id);
	public void update(User user, PatchUserRequest request);
}
```

### Service Interface
Defines the interface for the service.

```java
package com.sethfuller.service;

import java.util.List;

import com.sethfuller.model.PatchUserRequest;
import com.sethfuller.model.User;

public interface UserService {
	public List<User> getUsers();
	public User getUser(Long userId);
	public void create(User user);
	public void delete(Long id);
	public void update(User user, PatchUserRequest request);
}
```

### Service Implementation

```java
package com.sethfuller.service;

import java.util.List;
import java.util.regex.Pattern;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import com.sethfuller.constants.AppConstants;
import com.sethfuller.model.PatchUserRequest;
import com.sethfuller.model.User;
import com.sethfuller.repository.UserRepository;

@Service
@Transactional
public class UserServiceImpl implements UserService {

	@Autowired
	private UserRepository repository;

	@Override
	public List<User> getUsers() {
		return repository.findAll();
	}

	@Override
	public User getUser(Long id) {
		return repository.findByUserId(id);
	}

	@Override
	public void create(User user) {
		repository.save(user);
	}

	@Override
	public void delete(Long id) {
		repository.deleteByUserId(id);
	}

	@Override
	public void update(User user, PatchUserRequest request) {
		validateForPatch(request);
		updateUser(user, request);
		repository.save(user);
	}

	private void validateForPatch(PatchUserRequest request) {
		if(request.getEmail() != null && request.getEmail().isBlank())
			throw new IllegalArgumentException("Email can not be blank");
		
		// verify regular expressions
		if(request.getEmail() != null && !Pattern.matches(AppConstants.EMAIL_REGEXPR, 
				request.getEmail()))
			throw new IllegalArgumentException("Email is not valid");
		
		if(request.getFirstName() != null && request.getFirstName().isBlank())
			throw new IllegalArgumentException("First name can not be blank");
		
		if(request.getLastName() != null && request.getLastName().isBlank())
			throw new IllegalArgumentException("Last name can not be blank");		
	}

	private void updateUser(User user, PatchUserRequest request) {
		// update the values that are present in the request
		if (request.getFirstName() != null)
			user.setFirstName(request.getFirstName());

		if (request.getLastName() != null)
			user.setLastName(request.getLastName());

		if (request.getEmail() != null)
			user.setEmail(request.getEmail());
	}
}

```
## Repository
|Annotation|Description|
|----|----|
|@Repository|This interface is implemented by Spring with all methods for CRUD and data retrieval|
||JpaRepository<User, Long> - This is a JPA Repository for the User class with the id being of type Long|
```java
package com.sethfuller.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import com.sethfuller.model.User;

@Repository
public interface UserRepository extends JpaRepository<User, Long>  {

	User findByUserId(Long userId);
	void deleteByUserId(Long userId);
}

```

## Model
|Annotation|Description|
|----|----|
|@Entity |Table in the Database|
|@JsonInclude(Include.NON_NULL)||
|@Id|The unique identifier for the table|
|@GeneratedValue(strategy = GenerationType.AUTO)|Use the default id generator|
|@JsonIgnore|When sending to the client do not include this field|
|@Column(length = 30)|Database column - length 30|
|@JsonProperty("first_name")|When the JSON property name is different than the Java property name|
|@NotEmpty(message = "The first name can not be null or empty")|Generate an error is the column is empty|
|@Pattern(regexp = AppConstants.EMAIL_REGEXPR, message = "Email must be valid")|Validate that the data received matches the pattern|
|@CreationTimestamp|This field will be filled when the entity is created|
|@JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "dd-MM-yyyy HH:mm:ss")|Format for the field|

```java
package com.sethfuller.model;

public class ErrorMessage {

	private int code;
	private String message;

	public ErrorMessage(int code, String message) {
		super();
		this.code = code;
		this.message = message;
	}

	public int getCode() {
		return code;
	}

	public void setCode(int code) {
		this.code = code;
	}

	public String getMessage() {
		return message;
	}

	public void setMessage(String message) {
		this.message = message;
	}
}

```

```java
package com.sethfuller.model;

import java.util.Date;

import javax.persistence.AttributeOverride;
import javax.persistence.AttributeOverrides;
import javax.persistence.Column;
import javax.persistence.Embedded;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;
import javax.persistence.Transient;
import javax.validation.constraints.Email;
import javax.validation.constraints.NotBlank;
import javax.validation.constraints.NotEmpty;
import javax.validation.constraints.NotNull;
import javax.validation.constraints.Pattern;

import org.hibernate.annotations.CreationTimestamp;
import org.hibernate.annotations.UpdateTimestamp;

import com.fasterxml.jackson.annotation.JsonFormat;
import com.fasterxml.jackson.annotation.JsonIgnore;
import com.fasterxml.jackson.annotation.JsonInclude;
import com.fasterxml.jackson.annotation.JsonInclude.Include;
import com.fasterxml.jackson.annotation.JsonProperty;
import com.sethfuller.constants.AppConstants;

@Entity
@JsonInclude(Include.NON_NULL)
public class User {
	
	@Id
	@GeneratedValue(strategy = GenerationType.AUTO)
	@JsonIgnore
	private Long userId;

	@Column(length = 30)
	@JsonProperty("first_name")
	@NotEmpty(message = "The first name can not be null or empty")
	private String firstName;

	@Column(length = 30)
	@JsonProperty("last_name")
	@NotEmpty(message = "The last name can not be null or empty")
	private String lastName;

	@Column(length = 30)
	// TODO: use regular expression to validate email addresses
	// @Email(message = "The email must be valid") 
	@Pattern(regexp = AppConstants.EMAIL_REGEXPR, message = "Email must be valid")
	private String email;

	@JsonProperty("creation_date")
	@CreationTimestamp
	@JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "dd-MM-yyyy HH:mm:ss")
	private Date creationDate;

	public User() {
	
	}
	
	public User(String firstName, String lastName, String email) {
		this.firstName = firstName;
		this.lastName = lastName;
		this.email = email;
	}

	public Date getCreationDate() {
		return creationDate;
	}

	public void setCreationDate(Date creationDate) {
		this.creationDate = creationDate;
	}

	public Long getUserId() {
		return userId;
	}

	public void setUserId(Long userId) {
		this.userId = userId;
	}

	public String getFirstName() {
		return firstName;
	}

	public void setFirstName(String firstName) {
		this.firstName = firstName;
	}

	public String getLastName() {
		return lastName;
	}

	public void setLastName(String lastName) {
		this.lastName = lastName;
	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}
}
```

```java
package com.sethfuller.model;

import com.fasterxml.jackson.annotation.JsonProperty;

public class PatchUserRequest {

	@JsonProperty("first_name")
	private String firstName;
	
	@JsonProperty("last_name")
	private String lastName;
	
	private String email;

	public String getFirstName() {
		return firstName;
	}

	public void setFirstName(String firstName) {
		this.firstName = firstName;
	}

	public String getLastName() {
		return lastName;
	}

	public void setLastName(String lastName) {
		this.lastName = lastName;
	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}
}
```

[Spring Boot Main Page](spring_boot.md)
