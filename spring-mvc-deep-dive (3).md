# Understanding Spring MVC — Architecture, IoC, DI and Request Flow

*A working reference for developers who want to actually understand what happens under the hood, not just memorize annotations.*

---

## Why this document exists

Most tutorials jump straight into `@Controller` and `@RequestMapping` without explaining *why* Spring works the way it does. That's fine until something breaks — a bean doesn't wire up, a page doesn't load, or a request just returns 404 and you don't know why.

This paper explains Spring MVC simply, in plain words, with pictures where they help:

- **IoC** — who creates your objects
- **DI** — how they get connected to each other
- **Request Flow** — what happens between a browser click and the page you see

### The basic idea in one picture

```mermaid
graph LR
    U((User / Browser)) -->|1. sends request| C[Controller]
    C -->|2. asks for data| M[Model / Service]
    C -->|3. picks a| V[View]
    V -->|4. sends back page| U
```

That's really all MVC is: **Model** holds data, **View** shows it, **Controller** connects the two. Everything below just explains how Spring builds and manages this for you automatically.

---

## Table of Contents

1. [The Core Idea Behind Spring: Inversion of Control](#1-the-core-idea-behind-spring-inversion-of-control)
2. [Dependency Injection — the mechanism that implements IoC](#2-dependency-injection--the-mechanism-that-implements-ioc)
3. [The Spring Container](#3-the-spring-container)
4. [What Spring MVC Actually Is](#4-what-spring-mvc-actually-is)
5. [The Request Flow — Step by Step](#5-the-request-flow--step-by-step)
6. [DispatcherServlet in Detail](#6-dispatcherservlet-in-detail)
7. [Controllers](#7-controllers)
8. [Model, ModelAndView, and Views](#8-model-modelandview-and-views)
9. [View Resolvers](#9-view-resolvers)
10. [Configuration Styles](#10-configuration-styles)
11. [Interceptors vs Filters](#11-interceptors-vs-filters)
12. [Exception Handling](#12-exception-handling)
13. [A Complete Working Example](#13-a-complete-working-example)
14. [Common Mistakes I See Often](#14-common-mistakes-i-see-often)
15. [Closing Notes](#15-closing-notes)
16. [References](#references)

---

## 1. The Core Idea Behind Spring: Inversion of Control

Before touching Spring MVC, it helps to understand one simple idea: **Inversion of Control (IoC)**.

In plain words: **normally, your code creates the objects it needs. With Spring, Spring creates them for you and hands them over.** That's the whole idea. "Inversion" just means the control of *who creates what* has flipped — from your code, to the framework.

```mermaid
graph TB
    subgraph "Without Spring"
        A1[UserService] -->|creates itself| A2[new UserRepository]
    end
    subgraph "With Spring / IoC"
        B0[Spring Container] -->|creates| B2[UserRepository]
        B0 -->|creates & gives it to| B1[UserService]
    end
```

In a normal Java program, an object is responsible for creating the things it depends on. Say a `UserService` needs a `UserRepository`. Without any framework, you'd write:

```java
public class UserService {
    private UserRepository userRepository = new UserRepository();
}
```

This looks harmless, but it means `UserService` is now tightly bound to one specific implementation of `UserRepository`. If you want to swap it for a mock in a test, or a different database implementation, you have to change this class. The class is *controlling* its own dependencies.

IoC flips this responsibility. Instead of the object creating its own dependencies, something external — a **container** — creates the objects and hands them the dependencies they need. The object no longer controls how its dependencies are constructed; that control has been inverted and handed to the framework.

```java
public class UserService {
    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

Now `UserService` just declares what it needs. Something else — Spring — decides what actual object to hand it. That "something else" is the **Spring IoC Container**.

That's it. That one shift — from "I create what I need" to "give me what I need" — is the foundation everything else in Spring is built on.

---

## 2. Dependency Injection — the mechanism that implements IoC

IoC is the *idea*. **Dependency Injection (DI)** is *how Spring actually does it* — the act of handing an object its dependencies instead of letting it build them itself. There are three common ways Spring does this:

### 2.1 Constructor Injection (recommended)

```java
@Service
public class UserService {

    private final UserRepository userRepository;

    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

This is the preferred approach in modern Spring applications. It makes dependencies explicit, allows fields to be `final`, and makes the class impossible to construct in an invalid state.

### 2.2 Setter Injection

```java
@Service
public class UserService {

    private UserRepository userRepository;

    @Autowired
    public void setUserRepository(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

Useful when a dependency is optional, but it means the object can exist temporarily in an incomplete state.

### 2.3 Field Injection (common, but discouraged)

```java
@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;
}
```

This is the one you'll see in the most tutorials because it's short. It's also the one most experienced Spring developers avoid in real projects, because it hides dependencies, makes unit testing harder (no way to inject a mock without reflection or a Spring context), and hides circular dependency problems until runtime.

**Rule of thumb:** constructor injection by default, setter injection for optional dependencies, field injection only in quick prototypes or test classes.

---

## 3. The Spring Container

Think of the **Spring Container** as a big box that holds every object your app needs, already built and ready to use. It does three simple jobs:

1. Reads your configuration (annotations, Java config, or old-style XML)
2. Creates the objects your app needs — these are called **beans**
3. Connects (injects) those beans into each other automatically

```mermaid
graph TB
    Config[Your Annotations / Config] --> Container[Spring Container]
    Container --> Bean1[UserController bean]
    Container --> Bean2[UserService bean]
    Container --> Bean3[UserRepository bean]
```

There are two container interfaces you'll encounter:

| Interface | Description |
|---|---|
| `BeanFactory` | The basic container. Lazy-loads beans, minimal features. Rarely used directly today. |
| `ApplicationContext` | Extends `BeanFactory`. Adds event handling, internationalization, AOP integration, and eager bean loading. This is what every real Spring application uses. |

A **bean** is just an object that Spring creates and manages for you, instead of you writing `new SomeClass()` yourself. You mark a class as a bean using annotations like `@Component`, `@Service`, `@Repository`, and `@Controller`. They all do basically the same thing — they just tell Spring (and other developers) what layer that class belongs to.

```java
@Component
public class EmailValidator { }

@Service
public class UserService { }

@Repository
public class UserRepository { }

@Controller
public class UserController { }
```

At startup, Spring scans your packages (`@ComponentScan`), finds these annotations, creates one instance of each (by default, beans are singletons), and stores them in the `ApplicationContext`. From that point on, whenever a class asks for a `UserRepository`, Spring hands over the exact same managed instance and wires it in automatically.

---

## 4. What Spring MVC Actually Is

Spring MVC is Spring's web framework. It's built on the classic **Model-View-Controller** pattern, and it sits right on top of the container we just talked about. Nothing new happens here — your controllers are beans, your services are beans, everything gets wired the same way. Spring MVC simply adds one extra job on top: taking an incoming web request and routing it to the right piece of Java code, then turning the result back into an HTTP response.

The three pieces of MVC map like this in a Spring app:

- **Model** — your data. Typically POJOs / DTOs / entities passed between layers.
- **View** — what gets rendered back to the client. Could be a JSP, Thymeleaf template, or (in REST apps) a JSON representation with no "view" at all.
- **Controller** — the class that receives the request, talks to the service layer, and decides what to return.

---

## 5. The Request Flow — Step by Step

This is the part most tutorials skip. Here's the simple version first, then the detailed diagram.

**In one line:** Browser sends a request → one servlet catches everything → it finds the right controller → controller talks to your service → a page (or JSON) gets sent back.

Here's the classic Spring MVC control flow diagram, showing the same idea as 10 numbered steps:

![Spring MVC Framework Control Flow Diagram](spring-mvc-flow-diagram.png)

Walking through the numbers in that picture:

1. **Request** — the browser sends a request. It goes straight to the `DispatcherServlet`, the single entry point for everything.
2. **DispatcherServlet → Handler Mapping** — it asks "which controller should handle this URL?"
3. **Handler Mapping → DispatcherServlet** — it replies with the matching controller.
4. **DispatcherServlet → Controller** — the request is forwarded to that controller.
5. **Controller → DispatcherServlet (Model and View)** — the controller does its work and hands back data (the Model) plus a logical view name.
6. **DispatcherServlet → View Resolver** — it asks "which actual file does this view name point to?"
7. **View Resolver → DispatcherServlet** — it replies with the real view (e.g. a `.jsp` file).
8. **DispatcherServlet ↔ View** — the view is rendered using the model data.
9. *(rendering completes and control returns to DispatcherServlet)*
10. **Response** — the final HTML/output is sent back to the browser.

Now here's the same flow again, but drawn as a sequence diagram with interceptors included, for a more complete technical picture:

```mermaid
sequenceDiagram
    participant Client
    participant DS as DispatcherServlet
    participant HM as HandlerMapping
    participant Interceptor
    participant Controller
    participant Service
    participant VR as ViewResolver
    participant View

    Client->>DS: HTTP Request (e.g. GET /users/5)
    DS->>HM: Which controller handles this?
    HM-->>DS: HandlerExecutionChain (Controller + Interceptors)
    DS->>Interceptor: preHandle()
    Interceptor-->>DS: proceed (true)
    DS->>Controller: invoke handler method
    Controller->>Service: call business logic
    Service-->>Controller: return result
    Controller-->>DS: return Model + logical view name (or @ResponseBody JSON)
    DS->>Interceptor: postHandle()
    DS->>VR: resolve logical view name
    VR-->>DS: actual View object
    DS->>View: render(model)
    View-->>Client: HTML / JSON response
    DS->>Interceptor: afterCompletion()
```

In plain words, walking through the diagram:

1. **Browser sends a request.** No matter what URL it is, it all lands on one gatekeeper servlet: the `DispatcherServlet`. There is only ever one entry point.
2. **DispatcherServlet asks "who handles this?"** It checks `HandlerMapping`, which looks through all your `@GetMapping` / `@PostMapping` annotations to find a match.
3. **Interceptors get a first look**, if you've added any (like a login check). They can allow the request through or stop it early.
4. **Your controller method runs.** It usually calls a service, which calls a repository — all wired together by DI, same as in Section 2.
5. **Controller returns a result.** Either a view name (for a web page), or straight JSON if it's a REST controller.
6. **ViewResolver turns the view name into a real file** (like `"userProfile"` → `userProfile.jsp`), unless it's a REST response, which skips this step.
7. **The final page (or JSON) is built and sent back** to the browser.
8. **Interceptors get a last look**, mainly for cleanup or logging.

The one thing to remember: **everything goes through a single entry point** — `DispatcherServlet`. That one fact explains most of how Spring MVC behaves.

---

## 6. DispatcherServlet in Detail

Think of `DispatcherServlet` as the **front desk of a hotel**. It doesn't cook your food or clean your room — it just listens to what you need and sends you to the right department. Technically, it's a regular Java Servlet (it extends `HttpServlet`), but it doesn't do any real work itself. It just directs traffic.

It's registered against a URL pattern, commonly `/`, meaning it intercepts every request coming into the application. In a Spring Boot app, you never register this yourself — Spring Boot auto-configures it for you. In a classic non-Boot Spring MVC app, you'd register it in `web.xml` or through a `WebApplicationInitializer`:

```java
public class MyWebAppInitializer implements WebApplicationInitializer {

    @Override
    public void onStartup(ServletContext container) {
        AnnotationConfigWebApplicationContext context =
            new AnnotationConfigWebApplicationContext();
        context.register(WebConfig.class);

        ServletRegistration.Dynamic dispatcher =
            container.addServlet("dispatcher", new DispatcherServlet(context));
        dispatcher.setLoadOnStartup(1);
        dispatcher.addMapping("/");
    }
}
```

The `DispatcherServlet` internally relies on a handful of collaborator beans (all of which you can customize or replace):

| Component | Job |
|---|---|
| `HandlerMapping` | Decides which controller method should handle the incoming URL |
| `HandlerAdapter` | Actually invokes that method, regardless of its exact signature |
| `HandlerExceptionResolver` | Catches exceptions thrown during handling and decides what response to send |
| `ViewResolver` | Converts a logical view name into an actual renderable `View` |
| `LocaleResolver` | Determines locale for i18n |
| `MultipartResolver` | Parses file upload requests |

---

## 7. Controllers

A controller is just a normal bean with an `@Controller` annotation on it. Its only job is to receive a request and decide what to do about it.

There are two flavors:
- **`@Controller`** — returns a view name (a web page)
- **`@RestController`** — returns data directly (usually JSON). It's really just `@Controller` + `@ResponseBody` combined.

```java
@Controller
@RequestMapping("/users")
public class UserController {

    private final UserService userService;

    @Autowired
    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping("/{id}")
    public String getUser(@PathVariable Long id, Model model) {
        model.addAttribute("user", userService.findById(id));
        return "userProfile"; // logical view name
    }
}
```

Versus the REST version, which most modern applications actually use:

```java
@RestController
@RequestMapping("/api/users")
public class UserRestController {

    private final UserService userService;

    public UserRestController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping("/{id}")
    public UserDto getUser(@PathVariable Long id) {
        return userService.findById(id); // serialized to JSON directly
    }
}
```

Key annotations you'll use constantly on controller methods:

- `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`, `@PatchMapping` — shorthand for `@RequestMapping(method = ...)`
- `@PathVariable` — pulls a value out of the URL path
- `@RequestParam` — pulls a value out of query parameters
- `@RequestBody` — deserializes the incoming request body (usually JSON) into a Java object
- `@ResponseBody` — serializes the return value directly into the response body
- `@ResponseStatus` — sets a specific HTTP status code on the response

---

## 8. Model, ModelAndView, and Views

In the traditional (non-REST) flow, a controller needs a way to pass data to whatever template engine renders the final page. That's the `Model`.

```java
@GetMapping("/dashboard")
public String dashboard(Model model) {
    model.addAttribute("username", "vishal");
    model.addAttribute("stats", statsService.getSummary());
    return "dashboard"; // maps to dashboard.jsp / dashboard.html
}
```

You can also bundle the model and the view name into a single object, `ModelAndView`, if you prefer:

```java
@GetMapping("/dashboard")
public ModelAndView dashboard() {
    ModelAndView mv = new ModelAndView("dashboard");
    mv.addObject("username", "vishal");
    return mv;
}
```

Both approaches end at the same place: the `DispatcherServlet` hands the logical view name off to a `ViewResolver`.

---

## 9. View Resolvers

Your controller returns a short string like `"dashboard"`. But that's not a real file — someone has to turn it into one. That's the `ViewResolver`'s only job: turn a short name into the actual template file to render.

```mermaid
graph LR
    A["return 'dashboard'"] --> B[ViewResolver]
    B --> C["/WEB-INF/views/dashboard.jsp"]
```

A common JSP-based configuration:

```java
@Bean
public ViewResolver viewResolver() {
    InternalResourceViewResolver resolver = new InternalResourceViewResolver();
    resolver.setPrefix("/WEB-INF/views/");
    resolver.setSuffix(".jsp");
    return resolver;
}
```

With this, returning `"dashboard"` from a controller resolves to `/WEB-INF/views/dashboard.jsp`.

If you're using Thymeleaf (very common in modern Spring Boot apps), Spring Boot auto-configures a `ThymeleafViewResolver` for you the moment `spring-boot-starter-thymeleaf` is on the classpath — no manual bean needed.

REST APIs skip this step completely, since `@ResponseBody` / `@RestController` bypasses view resolution and writes straight to the HTTP response via an `HttpMessageConverter` (Jackson, by default, for JSON).

---

## 10. Configuration Styles

Spring MVC can be configured three ways. You'll mostly see the second and third today, but you'll still run into legacy XML in older codebases.

**XML-based (legacy):**

```xml
<context:component-scan base-package="com.example.app" />
<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/views/" />
    <property name="suffix" value=".jsp" />
</bean>
```

**Java-based config (`@Configuration`):**

```java
@Configuration
@EnableWebMvc
@ComponentScan("com.example.app")
public class WebConfig implements WebMvcConfigurer {

    @Bean
    public ViewResolver viewResolver() {
        InternalResourceViewResolver resolver = new InternalResourceViewResolver();
        resolver.setPrefix("/WEB-INF/views/");
        resolver.setSuffix(".jsp");
        return resolver;
    }
}
```

**Spring Boot (auto-configured, what most new projects use):**

```java
@SpringBootApplication
public class MyApp {
    public static void main(String[] args) {
        SpringApplication.run(MyApp.class, args);
    }
}
```

Spring Boot auto-configures the `DispatcherServlet`, an embedded Tomcat server, sensible default view resolvers, and Jackson for JSON — all based on what's on your classpath. You only override what you need to.

---

## 11. Interceptors vs Filters

These two get confused constantly, so it's worth being precise.

- **Servlet Filters** live outside Spring entirely — they're part of the Servlet spec, run before the request even reaches `DispatcherServlet`, and have no knowledge of which controller will eventually handle the request. Good for cross-cutting concerns like CORS headers or request logging at the raw HTTP level.

- **HandlerInterceptors** live inside Spring MVC, run *after* `DispatcherServlet` has already picked a handler, and can hook into three points: `preHandle` (before the controller runs), `postHandle` (after the controller runs, before the view renders), and `afterCompletion` (after the view has rendered, for cleanup). Good for things like authentication checks or measuring how long a specific controller took.

```java
public class AuthInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        if (request.getHeader("Authorization") == null) {
            response.setStatus(401);
            return false; // stops the chain here — controller never runs
        }
        return true;
    }
}
```

---

## 12. Exception Handling

Two common approaches, and you'll usually see the second one in real projects:

**Per-controller, with `@ExceptionHandler`:**

```java
@Controller
public class UserController {

    @ExceptionHandler(UserNotFoundException.class)
    public ResponseEntity<String> handleNotFound(UserNotFoundException ex) {
        return ResponseEntity.status(404).body(ex.getMessage());
    }
}
```

**Application-wide, with `@ControllerAdvice`:**

```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(UserNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleNotFound(UserNotFoundException ex) {
        ErrorResponse error = new ErrorResponse(ex.getMessage());
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGeneric(Exception ex) {
        return ResponseEntity.status(500).body(new ErrorResponse("Something went wrong"));
    }
}
```

`@ControllerAdvice` lets you centralize error handling instead of repeating the same `@ExceptionHandler` block in every controller — one class catches exceptions thrown from anywhere in the application.

---

## 13. A Complete Working Example

Putting the whole flow together — controller, service, repository, all wired through DI:

```java
// --- Repository layer ---
@Repository
public class UserRepository {
    public User findById(Long id) {
        // in reality: JPA / JDBC lookup
        return new User(id, "Vishal");
    }
}

// --- Service layer ---
@Service
public class UserService {

    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public User getUser(Long id) {
        return userRepository.findById(id);
    }
}

// --- Controller layer ---
@RestController
@RequestMapping("/api/users")
public class UserController {

    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        return ResponseEntity.ok(userService.getUser(id));
    }
}
```

Nothing here calls `new UserService()` or `new UserRepository()` manually. Spring's IoC container creates all three beans at startup, sees that `UserController` needs a `UserService` and `UserService` needs a `UserRepository`, and wires them together automatically through constructor injection — this is IoC and DI, working exactly as described back in Sections 1 and 2, just now inside a real request-handling application.

The dependency graph looks like this:

```mermaid
graph LR
    A[UserController] -->|depends on| B[UserService]
    B -->|depends on| C[UserRepository]
    D[ApplicationContext] -.creates & injects.-> A
    D -.creates & injects.-> B
    D -.creates & injects.-> C
```

---

## 14. Common Mistakes I See Often

- **Overusing field injection**, then wondering why unit tests need a full Spring context just to test one class.
- **Putting business logic inside controllers** instead of pushing it down into the service layer — makes controllers hard to test and reuse.
- **Forgetting `@ResponseBody` when mixing view-based and REST endpoints** in the same controller, and getting confused why Spring tries to resolve a JSON string as a view name.
- **Not understanding singleton scope** — by default every bean is a single shared instance. If you store per-request state in an instance field of a `@Service`, you'll get very confusing bugs under concurrent load.
- **Circular dependencies** between two services that both need each other — constructor injection will fail loudly at startup, which is actually a good thing; it forces you to redesign rather than silently limping along.

---

## 15. Closing Notes

Spring MVC looks big, but it really just answers two questions:

1. **Who builds my objects and connects them?** → Answered by IoC and DI.
2. **How does a web request find the right code and come back as a response?** → Answered by `DispatcherServlet`, `HandlerMapping`, and `ViewResolver`.

Once those two ideas click, everything else — interceptors, exception handling, templates — is just extra detail on top of a small, simple core.

If you ever get stuck debugging something in Spring MVC, go back to the diagram in Section 5 and ask: *which step is failing?* That usually solves it fast.

---

## References

1. Spring Framework Documentation — Web MVC Framework  
   https://docs.spring.io/spring-framework/reference/web/webmvc.html

2. Spring Framework Documentation — IoC Container  
   https://docs.spring.io/spring-framework/reference/core/beans.html

3. Spring Framework Documentation — Dependency Injection  
   https://docs.spring.io/spring-framework/reference/core/beans/dependencies.html

4. Spring Boot Reference Documentation  
   https://docs.spring.io/spring-boot/documentation.html

5. Baeldung — Spring MVC Tutorial  
   https://www.baeldung.com/spring-mvc-tutorial

6. Baeldung — Spring Dependency Injection  
   https://www.baeldung.com/spring-dependency-injection

7. Spring Guides — Building a RESTful Web Service  
   https://spring.io/guides/gs/rest-service

8. Java Servlet Specification (background on `HttpServlet` / `DispatcherServlet`)  
   https://jakarta.ee/specifications/servlet/

---

*Feel free to fork this, correct anything inaccurate, or extend it with Spring Security / Spring Data sections — happy to build those out as a follow-up.*
