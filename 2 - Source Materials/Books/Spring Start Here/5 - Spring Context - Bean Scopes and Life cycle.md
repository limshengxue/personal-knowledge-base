2025-07-12 14:01

# 5 - Spring Context - Bean Scopes and Life cycle
- 2 common use approach for creating beans and managing their lifecycles : Singleton and Prototype

## Singleton
- The default approach
- You always get the same instance when you refer to a specific bean
- But we can have multiple instance if they have different name
- The singleton used by Spring is *unique per name* instead of *unique per app*

### In Real World
- Singleton cause multiple components to share an object instance
- Thus, the singleton bean *must be immutable*
- If the singleton bean is mutable, it can cause race condition especially in multi-threading environment like web app

### Instantiation
- By default, Spring use *eager instantiation* which create all the beans when the context get initializes
- It bring 2 benefits
	- Early discovery of issue - error can trigger when instantiation (when app start)
	- Performance - bean is ready when required
- We use `@Lazy` annotation on the component to use *lazy instantiation*
	- Use when a specific client did not use a big part of the functionality

## Prototype
- Spring create a new instance every time we request a bean
- We need the annotation `@Scope` to change the bean scope
- Mutable bean is not a problem

### Using Prototype Bean
- When a Singleton Bean need to depends on a Prototype Bean, we must be careful *not to do DI for the Prototype bean*
- As the singleton bean is only instantiated once, the injection will only perform once, therefore we get same prototype object for all references of the singleton bean
- The correct way is usually requesting for the bean inside the singleton bean
- We can inject the `ApplicationContext` to do so

```java
@Service 
public class CommentService { 

@Autowired private ApplicationContext context; 

public void sendComment(Comment c) { 
	CommentProcessor p = context.getBean(CommentProcessor.class); 
	p.setComment(c); 
	p.processComment(c); 
	p.validateComment(c); 
	c = p.getComment(); // do something further 
	}
}
```



 

# References
