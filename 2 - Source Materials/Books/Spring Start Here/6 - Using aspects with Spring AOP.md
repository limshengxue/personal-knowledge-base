2025-07-12 19:25

# 6 - Using aspects with Spring AOP
- Aspects are a way the framework intercepts method calls and possibly alters the execution of methods
- This technique extract part of the logic belonging to the executing method
- For example, extract the logging code from the method and remain only business logic
- Besides that, aspects is a way for us to utilise the functionalities provided by Spring (e.g. transnationality, security)

## How Aspects work in Spring
- An aspect is simply a piece of logic the framework executes when you call specific method of your choice
- When designing aspects, consider
	- Aspect - What code you want Spring to execute when you call specific method
	- Advice - When the code should be executed (before/after/instead of the method)
	- Pointcut - which method need to be intercepted for aspect execution
	- Join point - event that trigger the execution of aspect (always a *method call* for Spring)
- We need the framework to manage the object which we want to apply aspects
- The bean which method will be intercepted are named *target object*

### How Spring implement Aspect
- Spring achieve AOP through *weaving*
- When a bean is the *target object*, Spring context return a proxy object instead of the "real object" when we request a reference to the bean
- When we call the method (point cut), the proxy applies the aspect logic and delegate the call to the actual method

## Implementing Aspects with Spring AOP
### Implementing a Simple Aspect
1. Enable aspect mechanism by annotating configuration class
```java
@Configuration  
@ComponentScan({"org.example.models", "org.example.services"})  
@EnableAspectJAutoProxy  
public class ProjectConfig {  
}
```
2. Create a new class, annotate with `@Aspect` annotation, add a bean for this class (using stereotypical annotation or `@Bean`)
```java
@Aspect  
@Component  
public class LoggingAspect {  
    public void log(){  
          
    }  
}
```

3. Define a method that will implement the aspect logic and tell Spring when and which methods to intercept using advice annotation
```java
@Aspect  
@Component  
public class LoggingAspect {  
    @Around("execution(* services.*.*(..))")  
    public void log(ProceedingJoinPoint joinPoint) throws Throwable {  
        joinPoint.proceed();  
    }  
}
```

In the `@Around` annotation, we define the which method to intercept. It use the AspectJ pointcut expression. In the example, it means intercept every method in the `services` package.
![[Attachments/Pasted image 20250712200405.png]]

4. Implement the aspect logic

```java
@Aspect  
@Component  
public class LoggingAspect {  
    private final Logger logger = Logger.getLogger(LoggingAspect.class.getName());  
  
    @Around("execution(* services.*.*(..))")  
    public void log(ProceedingJoinPoint joinPoint) throws Throwable {  
        logger.info("Method will execute");  
        joinPoint.proceed();  
        logger.info("Method executed");  
  
    }  
}
```
- The `joinPoint.proceed()` method invoke the actual method.
- Depends on the logic, we can actual not invoke it (e.g. authorisation failed)

### Altering the parameters and return value
- Accessing the parameters and return value
```java
@Around("execution(* org.example.services.*.*(..))") 
public Object log(ProceedingJoinPoint joinPoint) throws Throwable {  
    String methodName = joinPoint.getSignature().getName();  
    Object[] args = joinPoint.getArgs();  
    logger.info("Method " + methodName + " will execute with parameters " + Arrays.asList(args));  
    Object returnValue = joinPoint.proceed();  
    logger.info("Method executed and return value " + returnValue);  
    return returnValue;  
  
}
```

- Modifying the parameters (passing the new argument into the `joinPoint.proceed()` method)
```java
@Around("execution(* org.example.services.*.*(..))")  
public Object log(ProceedingJoinPoint joinPoint) throws Throwable {  
    String methodName = joinPoint.getSignature().getName();  
    Object[] args = joinPoint.getArgs();  
    logger.info("Method " + methodName + " will execute with parameters " + Arrays.asList(args));  
  
    // Changing the argument to the method  
    Comment newComment = new Comment();  
    newComment.setText("Another message");  
    Object[] newArgs = {newComment};  
    Object returnValue = joinPoint.proceed(newArgs);  
  
    logger.info("Method executed and return value " + returnValue);  
    return returnValue;  
  
}
```

This allow aspect to interfere the logic of the method by
- Altering the parameters
- Altering the return value
- Throwing an exception or handle an exception throw by the method
However, the guidelines is do not handle something no-obvious as it will make the code less maintainable.

### Intercepting Annotated Method
- We can define custom annotation to only intercept the annotated method
1. Define the custom annotation
```java
@Retention(RetentionPolicy.RUNTIME)  //Important to allow inteception in runtime
@Target(ElementType.METHOD)  
public @interface ToLog {  
}
```

2. Annotate the method that need to be intercepted
```java
@Service  
public class CommentService {  
    private final Logger logger =  Logger.getLogger(CommentService.class.getName());  
      
    @ToLog  
    public String publishComment(Comment comment){  
        logger.info("Publishing comment: " + comment.getText());  
        return "SUCCESS";  
    }  
  
    public String deleteComment(Comment comment){  
        logger.info("Deleting comment: " + comment.getText());  
        return "SUCCESS";  
    }  
}
```

3. Weave the annotation to the aspect method
```java
@Around("@annotation(org.example.ToLog)")  
public Object log(ProceedingJoinPoint joinPoint) throws Throwable {  
    String methodName = joinPoint.getSignature().getName();  
    Object[] args = joinPoint.getArgs();  
    logger.info("Method " + methodName + " will execute with parameters " + Arrays.asList(args));  
  
    // Changing the argument to the method  
    Comment newComment = new Comment();  
    newComment.setText("Another message");  
    Object[] newArgs = {newComment};  
    Object returnValue = joinPoint.proceed(newArgs);  
  
    logger.info("Method executed and return value " + returnValue);  
    return returnValue;  
  
}
```

### Other Advice Annotations
- `@Around` - the most commonly used, the most flexible
- `@Before` - calls the aspect method before the execution of the intercepted method
- `@AfterReturning` - After the intercepted method successfully return, provides the returned value as parameter (isn't call when intercepted method throw an Exception)
- `@AfterThrowing` - after intercepted method throw an exception
- `@After` - After intercepted method execution, no matter it return a value or threw an exception

## Aspect Execution Chain
- When there is multiple aspects, they need to execute one after another
- By default, Spring guarantee the order
- If we need specific order, we need the `@Order` annotation
- The annotation receive a number, the smaller, the earlier the aspect executes
```java
@Aspect 
@Order(1) 
public class SecurityAspect {
```



# References
