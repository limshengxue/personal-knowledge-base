2025-07-12 12:14

# 4 - Spring Context - Using Abstractions
## Using Interfaces to Define Contracts
- In Java, Interface is an abstract structured used to declare a specific responsibility
- Interface - define what need to happen
- Implementation - define how it should happen
- The main purpose of Interface is to achieve *decoupling*

### Using Interface for Decoupling Implementations
- In OOP, it is common to have an object delegate the responsibility to another object
- This create dependency
- For example, a `DeliveryDetailsPrinter` class might depends on a `SorterByAddress` class to sort the delivery details before printing
- However, depending on a concrete implementation bring *tight coupling*
- For example, when we need to sort by name instead of object, the code in `DeliveryDetailsPrinter` also need to be changed
- We can solve this problem by creating a `Sorter` interface
- The printer only need to invoke the sort method without being affected by its concrete implementation
```java
public class Main {  
    public static void main(String[] args) {  
        CommentNotificationProxy commentNotificationProxy = new EmailCommentNotificationProxy();  
        CommentRepository commentRepository = new DBCommentRepository();  
  
        CommentService service = new CommentService(commentRepository, commentNotificationProxy);  
        Comment newComment = new Comment();  
        newComment.setAuthor("James");  
        newComment.setText("Hello World");  
        service.publishComment(newComment);  
    }  
}
```

## Using Spring to Manage Dependency
- The data object `Comment` does not required to be added into the context
- We add `@Component` to the concrete implementation as they are the object that need to be instantiated by Spring context (not the Interfaces)
- The Service class need to be added with the `@Component` as well, meanwhile `@Autowired` is optional as there is only 1 constructor
- Setup the `ConfigurationClass` to scan for the components in their respective packages
- We can then let Spring context create the object and inject the dependencies for us
```java
public class Main {  
    public static void main(String[] args) {  
        var context = new AnnotationConfigApplicationContext(ProjectConfiguration.class);  
        CommentService service = context.getBean(CommentService.class);  
        Comment newComment = new Comment();  
        newComment.setAuthor("James");  
        newComment.setText("Hello World");  
        service.publishComment(newComment);  
    }  
}
```

### Multiple Implementations
- There are 2 main ways to help resolve the ambiguity when there is multiple implementations
	- `@Primary` annotation
	- `@Qualifier` annotation to match the bean with name

## Focusing on Object Responsibility with Stereotype Annotations
- It is common to define the component's purpose along with the stereotype annotation
- Using `@Component` is a generic annotation
- The specific annotations are like
	- `@Service` - mark component that takes the responsibility of a service
	- `@Repository`


# References
