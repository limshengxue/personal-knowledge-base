2025-07-06 09:56

# 2 - Spring Context - Defining Beans
- By default, Spring have no info about the objects that we create
- We add objects into context for Spring to see them
- Context is a complex mechanism that allow Spring to *manage our object*
- *Plugging features* that provided by Spring is done through context by adding object instances and establishing relationships among them.
- Object manage by Spring context is called *beans*

## Adding New Beans to Spring Context
### Creating the Context
- There are multiple implementations for the context
- A commonly used is `AnnotationConfigApplicationContext` which uses the annotation approach

### 1.0 Annotation Approach
- Step1: Define a Configuration Class in the Project
```java
@Configuration  
public class ProjectConfig {  
}
```
- Step2: Create a method that returns the bean with annotation
```java
@Configuration  
public class ProjectConfig {  
    @Bean  
    Parrot getParrot(){  
        Parrot parrot = new Parrot();  
        parrot.setName("Koko");  
        return parrot;  
    }  
}
```
- Step3: Create the Context instance with the Configuration class
```java
public class Main {  
    public static void main(String[] args) {  
        var context = new AnnotationConfigApplicationContext(ProjectConfig.class);  
        Parrot parrot = context.getBean(Parrot.class);  
        System.out.println(parrot.getName()); //Output: Koko 
  
    }  
}
```


What if we have multiple instance under same class
- We need to define the bean name (by default the method name is used as bean name)
```java
@Configuration  
public class ProjectConfig {  
    @Bean(name="parrot1")  
    Parrot getParrot(){  
        Parrot parrot = new Parrot();  
        parrot.setName("Koko");  
        return parrot;  
    }  
    @Bean(name="parrot2")  
    Parrot getParrot2(){  
        Parrot parrot = new Parrot();  
        parrot.setName("Momo");  
        return parrot;  
    }  
}
```

We then used the name as identifier when getting the instance from context
```java
Parrot parrot1 = context.getBean("parrot1",Parrot.class);  
Parrot parrot2 = context.getBean("parrot2",Parrot.class);
```

Or we can use the `@Default` annotation, Spring context will return the primary bean when `name` is not specified
```java
@Configuration  
public class ProjectConfig {  
    @Bean(name="parrot1")  
    @Primary  
    Parrot getParrot(){  
        Parrot parrot = new Parrot();  
        parrot.setName("Koko");  
        return parrot;  
    }  
    @Bean(name="parrot2")  
    Parrot getParrot2(){  
        Parrot parrot = new Parrot();  
        parrot.setName("Momo");  
        return parrot;  
    }  
}
```

### 2.0 Using Stereotype Annotations
- Write less code to instruct the adding of object into context
- Add `@Component` on the class that need to have an instance in Spring context
- This marked the class as component
- When the context get created, the instance of the class will get created and added into the context
```java
@Component  
public class Parrot {  
    private String name;  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
}
```
- We need to also tell Spring where to find the Component with `ComponentScan`
- Without argument it just scan the current package
```java
@Configuration  
@ComponentScan  
public class ProjectConfig {  
  
}
```
- If we need initialization code after the instance was created by the context, we use the `@PostConstruct` annotation from Jakarta EE
```java
@Component  
public class Parrot {  
    private String name;  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    @PostConstruct  
    public void postConstruct(){  
        this.name = "Koko";  
    }  
  
}
```

### 1.0 vs 2.0
- `@Bean` annotation
	- Provide full control on creating and configuring instance
	- Add more instances of same type
	- Can work on any instance even when the class file is not available
	- Cons: Require defining a method for each type of bean
- Stereotype annotations
	- Only have control after the framework create the instance
	- 1 instance per class
	- Can only used when the class file available
	- No extra boilerplate

### 3.0 Add Object into Context Programmatically
- Suitable when we want to register 
- The `registerBean` require 4 arguments
	- bean name
	- class of the bean
	- a supplier, an implementation of the functional interface `Supplier`, provides control to how the instance will be created, expect no parameter and return the instance 
	- `BeanDefinitionCustomizer`, allow customize the bean property (e.g. set to primary)
```java
var context = new AnnotationConfigApplicationContext(ProjectConfig.class);  
  
context.registerBean("parrot", Parrot.class, ()->{  
    Parrot p = new Parrot();  
    p.setName("Koko");  
    return p;  
}, bc -> bc.setPrimary(true));  
  
Parrot parrot = context.getBean(Parrot.class);  
  
System.out.println(parrot.getName());
```

# References
