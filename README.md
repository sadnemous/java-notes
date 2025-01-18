### java-notes
#### 1. @Component : 
  `@Component` annotation declares a class as a Spring component. 
  This signals to Spring that the class should be managed by the Spring container, meaning it will be automatically 
  - created, 
  - initialized, and 
  - potentially injected 
  into other components.

#### 2. application.properties
  Instead of different `application-<env>.properties` file we should create `application.yml` file. Sample `application.yml` can be 
  ```yml
spring:
   application:
      name: demoservice
server:
   port: 8080

---
spring:
   profiles: dev
   application:
      name: demoservice
server:
   port: 9090

---
spring: 
   profiles: prod
   application:
      name: demoservice
server: 
   port: 4431
  ```
Say jar file is `demo-0.0.1-SNAPSHOT.jar` and `environment=prod`<br>
to run it<br>
```bash
  $ java -jar demo-0.0.1-SNAPSHOT.jar --spring-profiles.active=prod
```

#### 3. @Configuration Annotation
In Spring Boot, the `@Configuration` annotation is used to indicate that a class is a source of bean definitions.<br>
It's a core annotation from the Spring Framework, not specific to Spring Boot.<br>
Classes marked with @Configuration can define beans using the @Bean annotation on methods within the class.<br>
These beans are then managed by the Spring container.<br>
`@Configuration` promotes a Java-based configuration approach instead of relying solely on XML configuration files.<br>
<img src="config.png">

Interesting notes from https://www.digitalocean.com/community/tutorials/spring-configuration-annotation
If we remove `@Configuration`, Config class will not be singleton..

#### 4. @RestController
`@RestController`:<br>
  - marks a class as controller for RESTful web requests. 
  - It combines the functionality of @Controller and @ResponseBody. Here's what it does:

`@Controller`
  − Marks the class as a Spring MVC controller
  - responsible for handling incoming HTTP requests.

`@ResponseBody`
 - Tells Spring to serialize the return value of the controller's method directly into the HTTP response body, typically in formats like JSON or XML.

#### 5. ResponseEntity:
In Spring Boot, ResponseEntity is a class that allows you to have fine-grained control over the HTTP response sent back to the client from your REST controllers. <br>
It provides a way to customize the status code, headers, and body of the response.

<b>Why use ResponseEntity?</b><br>
Custom Status Codes:
You can return specific HTTP status codes to indicate the outcome of an operation, <br>
such as 
- 200 (OK), 
- 201 (Created), 
- 404 (Not Found), 
- 500 (Internal Server Error), etc.<br>

Custom Headers:<br>
Add additional headers to the response to provide further information, <br>
such as caching directives or authentication tokens.<br>


Flexibility in Response Body:
Return different types of data in the response body, including JSON, XML, plain text, or even custom objects.
Example:
```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class UserController {

    @GetMapping("/users/{id}")
    public ResponseEntity<User> getUserById(Long id) {
        User user = userService.findById(id);

        if (user != null) {
            return new ResponseEntity<>(user, HttpStatus.OK);
        } else {
            return new ResponseEntity<>(HttpStatus.NOT_FOUND);
        }
    }
}
```
<b>Explanation:</b><br>
The getUserById method attempts to retrieve a user by ID.<br>
If the user is found, it returns a ResponseEntity with the user object and a 200 OK status code.<br>
If the user is not found, it returns a ResponseEntity with a 404 Not Found status code.<br>


When to use ResponseEntity?<br>
When you need to customize the HTTP status code beyond the standard 200 OK.<br>
When you need to add extra headers to the response.<br>
When you want to return different types of data in the response body based on the outcome of an operation.<br>


Alternative:<br>
If you don't need to customize the response, you can simply return the object from your controller method, and Spring will handle the rest.<br>
```java
@GetMapping("/users")
public List<User> getAllUsers() {
    return userService.findAll();
}
```





