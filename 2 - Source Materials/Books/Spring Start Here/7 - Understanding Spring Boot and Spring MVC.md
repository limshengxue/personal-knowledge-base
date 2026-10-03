2025-07-13 12:31

# 7 - Understanding Spring Boot and Spring MVC
## What is Web App
- Web app composed of 2 parts
	- Client side/frontend - user interacts with directly, the web browser. The browser sends requests to a web server and receive responses from it.
	- Server side/backend - implements logic that processes and sometimes stores the client requested data.

### 2 main design
1. Apps where backend provides the fully prepared view in response to client's request. The browser directly interprets the data from backend and display this information.
2. Apps using frontend-backend separation. Backend serves raw data. The browser run a separate frontend app that process the data and instructs the browser what to display. Sometimes referred to as the modern approach.

### Using Servlet Container
- Web browser uses a protocol named HTTP to communicate with the server over the network
- What we need to build a web server is something that understands HTTP and translate HTTP request and response to a Java App
- Servlet Container will do this job - as a translator between HTTP messages and Java app
- A widely used is Tomcat
- *Servlet* is a Java object that handle a HTTP request sent to a specific path
- The *Servlet container* manages Servlets, when it receive a request, it calls the servlet's method and pass the request as the parameter, the response object is also a parameter
- Sometime ago, creating servlet is important, for each path the response will get sent to, the developer register a servlet and register to the servlet container
- But with Spring, we don't have to worry about that

## The magic of Spring Boot
- To create Spring web app, we need to
	- Configure Servlet container
	- Create Servlet instance
	- Configure the instance such that the container calls its for any client request (as an entry point of the app)
- Spring boot simplify this process using the *convention over configuration* approach
	- Simplified project creation - using project initialization service to get an empty skeleton
	- Dependency starters - group dependencies based on purpose
	- Autoconfiguration based on dependencies - provide default configurations based on dependencies added

### Initialization Service
- Accessible via `start.spring.io` can add dependencies
- Provide
- The main class
- Spring Boot POM parent
It ensures the dependencies added later to be compatible
We should not define the dependencies version but let Spring boot help us to search for the compatible one
```java
<parent>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-parent</artifactId>  
    <version>3.5.3</version>  
    <relativePath/> <!-- lookup parent from repository -->  
</parent>
```
- Maven plugin for Spring Boot
This plugin specify the default configuration for the app
```java
<build>  
    <plugins>       <plugin>          <groupId>org.springframework.boot</groupId>  
          <artifactId>spring-boot-maven-plugin</artifactId>  
       </plugin>    </plugins></build>
```
- Dependencies

### Dependency Starters
- Group of dependencies added to configure the app for a specific purpose
- We request capabilities instead of dependencies with Spring Boot
- Spring Boot adds the require dependencies and ensure their versions are compatible
```java
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-web</artifactId>  
</dependency>
```

## Creating a Simple Web App
Defining the webpage in `resources.static` folder
```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Home Page</title>  
</head>  
<body>  
    <h1>Welcome!</h1>  
</body>  
</html>
```

Defining the controller
- The controller is a web app component that contains method (often named actions) executed for a specific HTTP request
- We mark controller with `@Controller` another stereotypical annotation like `@Service` , `@Component`
 ```java
 @Controller  
public class HomePageController {  
  
    @RequestMapping("/home")  
    public String homepage(){  
        return "homepage.html";  
    }  
}
```

## How Spring MVC Works
- Tomcat accept the request and call the *dispatcher servlet* (provided by Spring)
- The *dispatcher servlet* is responsible for finding out which method of the controller to call. This servlet also called *front controller*
- It delegate this to a *handler mapping* component
- The dispatcher then calls the found method, if not found, 404 error was returned
- The HTML return by the action method is called *the view*
- To get the view content using the view name and render it with the data, dispatcher servlet use *view resolver*

![[Attachments/Pasted image 20250713140004.png]]


# References
