# Building a RESTful Web Service with Spring Boot
Based on this [Spring Guide](https://spring.io/guides/gs/rest-service/)

API service that will accept HTTP GET requests at: 
```http://localhost:8080/greeting```

and respond with a JSON representation of a greeting:
```{"id":1,"content":"Hello, World!"}```

The greeting can be customized with an optional name parameter in the query string:
```http://localhost:8080/greeting?name=User```
which returns:
```{"id":1,"content":"Hello, User!"}```

### Steps Taken to Create The Project
1. Create a new Gradle Project via IntelliJ
    - New Project
    - Gradle w Java
    - Select domain specific GroupId & artifactId (usually project name)
    - Select 'Use default gradle wrapper' & 'Create directories for empty content root automatically'
2. Define a package directory with the source directory
3. Test and create a resource controller
   - This is where the Spring Boot magic (not magic; just annotations) happens... 
   - What will your endpoint be? Query params? HTTP verbs?
   - What are your inputs & outputs? And what model objects do you need?
4. Create a model (aka resource representation class)
   - This is a POJO to represent the resource that will be served up by the controller
5. Create a 'Application' class 
   - This is where the ```main()``` method lives, which creates a single, executable JAR file. 
   - Along the way, you use Spring’s support for embedding the Tomcat servlet container as the HTTP runtime, instead of deploying to an external instance.



### Annotations
#### GreetingController.java
- @RestController annotation marks the class as a controller where every method returns a domain object instead of a view. 
    - It’s shorthand for @Controller and @ResponseBody rolled together.         
- @RequestMapping annotation ensures that HTTP requests to /greeting are mapped to the greeting() method.
    - note: @RequestMapping maps all HTTP operations by default. Use @RequestMapping(method=GET) to narrow this mapping.
- @RequestParam  binds the value of the query string parameter name into the name parameter of the greeting() method. 
    - This query string parameter is explicitly marked as optional (required=true by default): if it is absent in the request, the defaultValue of "World" is used.
    - The Greeting object must be converted to JSON. Thanks to Spring’s HTTP message converter support, you don’t need to do this conversion manually. 
    - Because Jackson 2 is on the classpath, Spring’s MappingJackson2HttpMessageConverter is automatically chosen to convert the Greeting instance to JSON.

#### HelloApplication.java
- @SpringBootApplication is a convenience annotation that adds all of the following                     
    - @Configuration tags the class as a source of bean definitions for the application context.
    - @EnableAutoConfiguration tells Spring Boot to start adding beans based on classpath settings, other beans, and various property settings.
    - Normally you would add @EnableWebMvc for a Spring MVC app, but Spring Boot adds it automatically when it sees spring-webmvc on the classpath. This flags the application as a web application and activates key behaviors such as setting up a DispatcherServlet.
    - @ComponentScan tells Spring to look for other components, configurations, and services in the hello package, allowing it to find the controllers.

