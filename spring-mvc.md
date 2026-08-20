## Spring Framework and Spring MVC
## What is Spring?
Spring is a Java framework for building applications, mostly web apps. It handles object creation, dependencies, database work, and security, so we don't have to write that code ourselves.

In normal Java, we write something like, UserRepository repo = new UserRepository();

In Spring, Spring creates and manages the object for us.
## IoC – Inversion of Control
Normally our own code creates objects, like: UserService service = new UserService();
In Spring, Spring creates that object for us instead. IoC just means Spring takes control of creating objects, instead of us doing it by hand.
## Dependency Injection
DI means Spring gives one object the other object it needs.

There are three types of DI: Constructor Injection, Setter Injection, and Field Injection. Constructor injection is usually the best pick, since it's simple and easy to test.

## Spring Container and Bean
The Spring Container manages all the objects used in the app. An object managed by Spring is called a Bean. Some common annotations  are Component, Service, Repository, and Controller. Spring sees these annotations, then creates and manages the object for us.

## Important Annotations
```java
@Component
@Service
@Repository
@Controller
@RestController
@Autowired
@GetMapping
@PostMapping
@PutMapping
@DeleteMapping
@PathVariable
@RequestParam
@RequestBody
```

Example:
```java
@RestController
public class EmployeeController {

    @GetMapping("/employees")
    public String getEmployees() {
        return "All Employees";
    }

    @PostMapping("/employees")
    public String addEmployee(@RequestBody Employee employee) {
        return "Employee Added";
    }
}
```
Component, Service, and Repository all create a bean.

- Service holds business logic.
- Repository handles database code.
- Controller returns a web page.
- RestController returns JSON.-Autowired injects a dependency.
- GetMapping handles GET requests.
- PostMapping handles POST requests.
- PathVariable reads a value from the URL.
- RequestParam reads a query parameter.
- RequestBody converts incoming JSON into a Java object.

## Spring MVC

### Simple MVC Example

```java
@RestController
public class EmployeeController {

    @GetMapping("/employees/{id}")
    public Employee getEmployee(@PathVariable int id) {
        return employeeService.getEmployee(id);
    }
}
```


Spring MVC builds web apps and REST APIs. MVC stands for Model, View, Controller. Model is the data, View is what the user sees, and Controller handles the request.

The general flow looks like this: Controller talks to Service, Service talks to Repository, and Repository talks to the Database.

## Controller, Service, Repository


```java
@RestController
public class EmployeeController {

    @GetMapping("/employees/{id}")
    public Employee getEmployee(@PathVariable int id) {
        return service.getEmployee(id);
    }
}
```

```java
@Service
public class EmployeeService {

    public Employee getEmployee(int id) {
        return repository.findById(id);
    }
}
```

```java
@Repository
public interface EmployeeRepository
        extends JpaRepository<Employee, Integer> {
}
```


The Controller receives the request and usually just calls the Service to do the real work.

The Service holds the actual business logic of the application.

The Repository handles the database work, like insert, select, update, and delete. Spring Data JPA already gives us many of these methods, so we don't need to write them ourselves.
## ViewResolver
In Spring MVC, a controller can return just a view name, like "home". ViewResolver then finds the real page for us, like home.jsp. REST APIs skip this step entirely since they return JSON directly.

## Main Flow
Spring's container creates beans and injects their dependencies. In Spring MVC, the client sends a request to the DispatcherServlet, which goes to the Controller, then the Service, then the Repository, then the Database, and finally the response comes back.


## Spring MVC Architecture

![Spring MVC Architecture](spring-mvc-flow-diagram.png)

## References

1\. Spring Framework Documentation — Web MVC Framework: [https://docs.spring.io/spring-framework/reference/web/webmvc.html](https://docs.spring.io/spring-framework/reference/web/webmvc.html)

2\. Spring Framework Documentation — IoC Container: [https://docs.spring.io/spring-framework/reference/core/beans.html](https://docs.spring.io/spring-framework/reference/core/beans.html)

3\. Baeldung — Spring MVC Tutorial: [https://www.baeldung.com/spring-mvc-tutorial](https://www.baeldung.com/spring-mvc-tutorial)