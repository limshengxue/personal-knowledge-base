2025-07-06 12:38

# 3 - Spring Context - Wiring Bean
- Wiring can be achieved 3 ways
	- Wiring: Link the beans by directly calling the methods that create them
	- Auto-Wiring: Enable Spring to provide us a value using a method parameter (dependency injection)

## Wiring
- Calling the method directly in Config class
```java
@Configuration  
public class ProjectConfig {  
    @Bean  
    public Parrot parrot(){  
        Parrot parrot = new Parrot();  
        parrot.setName("Koko");  
        return parrot;  
    }  
  
    @Bean  
    public Person person(){  
        Person person = new Person();  
        person.setParrot(parrot()); // Wiring 
        return person;  
    }  
}
```
- We need to know that parrot is created only once even when we requested parrot
```java
var context = new AnnotationConfigApplicationContext(ProjectConfig.class);  
Parrot p = context.getBean(Parrot.class);  
// Parrot created
Person person = context.getBean(Person.class);  
// Parrot reused
```

## Auto-Wiring
- Depends on Spring to provide the value
- Can be used for both bean creation approach (`@Bean` or the `@Component` )
Using with `@Bean`
```java
@Configuration  
public class ProjectConfig {  
    @Bean  
    public Parrot parrot(){  
        Parrot parrot = new Parrot();  
        parrot.setName("Koko");  
        return parrot;  
    }  
  
    @Bean  
    public Person person(Parrot parrot){ //Auto-wiring
        Person person = new Person();  
        person.setParrot(parrot);  
        return person;  
    }  
}
```
- This mechanism is known as *dependency injection*, which is an application of IoC principle
- It means the IoC container inject the value required through method parameters

### Using `@Autowired` with `@Component`
#### Injecting through class field
- Rarely used in production-level, mostly in PoC, tests, or examples
- Pros: very convenient to use
- Cons: 
	- Cannot manage the instance during initialization
	- Cannot make the field final
```java
@Component  
public class Person {  
    private String name;  
    @Autowired  
    private Parrot parrot;  
}
```

#### Injecting through constructor
- Commonly used in production and most recommended
- Allow final field
```java
@Component  
public class Person {  
    private String name;  
    private final Parrot parrot;  
  
    @Autowired  
    public Person(Parrot parrot){  
        setParrot(parrot);  
    }
```

#### Injecting through setter
- Not recommended
- Low readability
- Cannot make field as final
```java
@Component 
public class Person { 
	private String name = "Ella"; 
	private Parrot parrot; // Omitted getters and setters 
	@Autowired public void setParrot(Parrot parrot) 
	{
	this.parrot = parrot; 
	} 
}
```

## Dealing with circular dependencies
- Circular dependencies happen when the creation of 1 object depends on another which creation depends on itself

## Choosing from multiple beans in the context
When there is multiple beans available for injection, we need to help Spring to decide which to inject by:
- Select the primary bean if defined
- Select based on the`@Qualifier` annotation (higher priority)
- If both is none, app fail with exception (Ambiguous injection)

Using `@Qualifier`
```java
@Component  
public class Person {  
    private String name;  
    private Parrot parrot;  
  
    public Person(@Qualifier("parrot2") Parrot parrot2){  
        setParrot(parrot2);  
    }
}
```

# References
