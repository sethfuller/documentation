# Spring Boot 3.0.x Documentation
[Documentation](https://docs.spring.io/spring-boot/docs/3.0.x/reference/html/index.html)

# @SpringBootApplication
[@SpringBootApplication](https://docs.spring.io/spring-boot/docs/2.0.x/reference/html/using-boot-using-springbootapplication-annotation.html)

The @SpringBootApplication annotation enables these annotations:

|Annotation|Description|
|----|---|
|@EnableAutoConfiguration| Enable Spring Boot’s auto-configuration mechanism|
|@ComponentScan| Enable @Component scan on the package where the application is located (see the best practices)|
|@Configuration| Allow to register extra beans in the context or import additional configuration classes|

# @RestController
Combines the @Controller and @ResponseBody annotations.

# @Component

## @Service
The business logic of an application usually resides within the
service layer, so we’ll use the @Service annotation to indicate that a
class belongs to that layer.

## @Repository
It has automatic persistence exception translation enabled. When using
a persistence framework, such as Hibernate, native exceptions thrown
within classes annotated with @Repository will be automatically
translated into subclasses of Spring’s DataAccessExeption.

# @Configuration
[@Configuration Example](https://www.geeksforgeeks.org/spring-configuration-annotation-with-example/)

# @ComponentScan
Spring can automatically scan a package for beans if component scanning is enabled.

@ComponentScan configures which packages to scan for classes with
annotation configuration. We can specify the base package names
directly with one of the basePackages or value arguments (value is an
alias for basePackages):

```java
@Configuration
@ComponentScan(basePackages = "com.baeldung.annotations")
class VehicleFactoryConfig {}
```

Also, we can point to classes in the base packages with the basePackageClasses argument:

```java
@Configuration
@ComponentScan(basePackageClasses = VehicleFactoryConfig.class)
class VehicleFactoryConfig {}
```

Both arguments are arrays so that we can provide multiple packages for each.

If no argument is specified, the scanning happens from the same
package where the @ComponentScan annotated class is present.

@ComponentScan leverages the Java 8 repeating annotations feature,
which means we can mark a class with it multiple times:

```java
@Configuration
@ComponentScan(basePackages = "com.baeldung.annotations")
@ComponentScan(basePackageClasses = VehicleFactoryConfig.class)
class VehicleFactoryConfig {}
```

Alternatively, we can use @ComponentScans to specify multiple @ComponentScan configurations:

```java
@Configuration
@ComponentScans({ 
  @ComponentScan(basePackages = "com.baeldung.annotations"), 
  @ComponentScan(basePackageClasses = VehicleFactoryConfig.class)
})
class VehicleFactoryConfig {}
```

# Aspect Oriented Programming
When we use Spring stereotype annotations, it’s easy to create a
pointcut that targets all classes that have a particular stereotype.

For instance, suppose we want to measure the execution time of methods
from the DAO layer. We’ll create the following aspect (using AspectJ
annotations), taking advantage of the @Repository stereotype:

```java
@Aspect
@Component
public class PerformanceAspect {
    @Pointcut("within(@org.springframework.stereotype.Repository *)")
    public void repositoryClassMethods() {};

    @Around("repositoryClassMethods()")
    public Object measureMethodExecutionTime(ProceedingJoinPoint joinPoint) 
      throws Throwable {
        long start = System.nanoTime();
        Object returnValue = joinPoint.proceed();
        long end = System.nanoTime();
        String methodName = joinPoint.getSignature().getName();
        System.out.println(
          "Execution of " + methodName + " took " + 
          TimeUnit.NANOSECONDS.toMillis(end - start) + " ms");
        return returnValue;
    }
}
```
