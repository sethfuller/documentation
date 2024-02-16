# HATEOAS
HATEOAS stands for Hypermedia as the Engine of Application State.

### Dependency
#### Maven

```xml
    <dependencies>
        ...
		<dependency>
	      <groupId>org.springframework.boot</groupId>
	      <artifactId>spring-boot-starter-hateoas</artifactId>
	   </dependency>
       ...
    </dependencies>
```
#### Gradle
```groovy
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-hateoas'
}
```

### Example
```java

import org.springframework.hateoas.EntityModel;
import org.springframework.hateoas.server.mvc.WebMvcLinkBuilder;

@RestController
public class CustomerController {

    @GetMapping("/customers")
    public List<Customer> getAllCustomers() {
        List<Customer> customerList = service.getAllCustomers();

        return customerList;
    }

    @GetMapping("/customers/{id}")
    public EntityModel<Customer> getCustomerById(@PathVariable id) {
        Customer customer = service.getCustomerById(id);

        if (customer == null) {
            throw CustomerNotFoundException("id: " + id);
        }
        EntityModel<Customer> entityModel = EntityModel.of(customer);

        WebMvcLinkBuilder link =
            WebMvcLinkBuilder.linkTo(WebMvcLinkBuilder.methodOn(this.getClass()).getAllCustomers());

        entityModel.add(link.withRel("all-customers"));

        return entityModel;
    }

}
```

The returned JSON for the request has the link (in the "_links" object) for all
customers. You can add more links to other endpoints if you want in the same way
as above.

The "_links" object is in HAL format (see below).

```json
    {
        "name": "",
        "addr1": "",
        "addr2": "",
        "city": "",
        "postalCode": "",
        "country": ""
        "_links": {
            "all-customers": {
                "href": "http://localhost:80080/customers"
            }
        }
    }
```

### Hypertext Application Language (HAL) Explorer
The best way to view HAL links is with HAL Explorer. To do that add the following to your
pom.xml.

#### Dependency
##### Maven
In the **pom.xml** file add the following starter dependency.

```xml
    <dependencies>
        ...
		<dependency>
			<groupId>org.springframework.data</groupId>
			<artifactId>spring-data-rest-hal-explorer</artifactId>
		</dependency>
       ...
    </dependencies>
```

##### Gradle
```groovy
dependencies {
    implementation 'org.springframework.data:spring-data-rest-hal-explorer'
}
```

#### URLs
To access the HAL Explorer enter the following URL in your browser:

```
    http://localhost:8080
```

This redirects you to:
```
    http://localhost:8080/index.html#url=/
```
