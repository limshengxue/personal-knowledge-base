2026-10-03 15:43

Tags: [[software architecture]]

# Spring MVC Request Lifecycle
- Spring MVC is Spring's servlet-based web framework.
- The servlet container manages HTTP connections and servlet execution.
- Spring's `DispatcherServlet` coordinates request handling; it is not the servlet container itself.

## Request Flow
1. The container forwards a matching request to `DispatcherServlet`.
2. Handler mappings select the controller method.
3. A handler adapter invokes the handler, resolving arguments and processing its result.
4. For a view response, model data and a view name are used to resolve and render a view.
5. For a response-body result, message conversion writes data directly to the HTTP response.

Interceptors, exception handling, and other framework components can participate around these steps. [DispatcherServlet processing](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-servlet/sequence.html).

## View Controller Example
This fragment assumes a configured template engine and a `welcome` template:

```java
@Controller
class WelcomeController {
  @GetMapping("/welcome")
  String welcome(Model model) {
    model.addAttribute("message", "Hello");
    return "welcome";
  }
}
```

- Returning `"welcome"` normally names a view; it does not automatically mean a literal response body.
- `@RestController` includes response-body semantics; return values are written through message converters instead of view resolution.
- JSON output depends on the chosen return type and installed converters. [Controller declarations](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann.html).

## Responsibility Boundaries
- Controllers translate HTTP input and output.
- Services own application behaviour and receive dependencies through [[Spring Dependency Injection]].
- Repositories handle persistence.
- Static resources can be served without a controller; a template view and a static HTML file are different mechanisms.

[[Spring Boot Fundamentals]] can configure common MVC infrastructure, but understanding this lifecycle helps diagnose routing, conversion, and template failures.

# References
[[2 - Source Materials/Books/Spring Start Here/7 - Understanding Spring Boot and Spring MVC|7 - Understanding Spring Boot and Spring MVC]]

