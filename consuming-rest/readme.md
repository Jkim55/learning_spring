# Consuming a RESTful Web Service with Spring Boot
Based on this[Spring Guide](https://spring.io/guides/gs/consuming-rest/)

Spring provides a convenient template class called RestTemplate to programatically consume a RESTful service. 
RestTemplate makes interacting with most RESTful services a one-line incantation and it can even bind that data to custom domain types.

This project will call a RESTful service accessed via a get to http://gturnquist-quoters.cfapps.io/api/random
The service randomly fetches quotes about Spring Boot and returns them as a JSON document that looks something like this:

```
{
   type: "success",
   value: {
      id: 10,
      quote: "Really loving Spring Boot, makes stand alone Spring apps easy."
   }
}
```


### Steps Taken to Create The Project
1. Create a new Gradle Project via IntelliJ & add dependencies within build.gradle
2. Create a Domain Class to contain the data you need
    - POJO created as Quote.java
    - Annotate it with ```@JsonIgnoreProperties``` from the Jackson JSON processing library to indicate that any properties not bound in this type should be ignored
    - In order for you to directly bind your data to your custom types, you need to specify the variable name exact same as the key in the JSON Document returned from the API. 
      In case your variable name and key in JSON doc are not matching, you need to use @JsonProperty annotation to specify the exact key of JSON document.
    - Note that a private property 'Value' is defined as a part of the Quote class. Thus, an additional class, Value, is  needed to embed the inner quotation itself.  
        
3. Create another pojo called Value to represent the embed the inner quotation in the Quote class 

4. Make the application executable via 'Application' class 
   - Spring Boot manage the message converters in the RestTemplate, so that customizations are easy to add declaratively. 
        - To do that we use @SpringBootApplication on the main class and convert the main method to start it up, like in any Spring Boot application
   - Move the RestTemplate to a CommandLineRunner callback so it is executed by Spring Boot on startup
        - The RestTemplateBuilder is injected by Spring; if you use it to create a RestTemplate then you will benefit from all the autoconfiguration that happens in Spring Boot with message converters and request factories. 
        - We also extract the RestTemplate into a @Bean to make it easier to test (it can be mocked more easily that way).

### Annotations
#### Quote.java
- @JsonIgnoreProperties(ignoreUnknown = true): Jackson JSON processing library to indicate that any properties not bound in this type should be ignored


#### Application.java
- @Bean
    - Used at the method level, the ```@Bean``` annotation works with ```@Configuration``` (which comes packaged in ``` @SpringBootApplication```) to create Spring beans. 
    - @Configuration will have methods to instantiate and configure dependencies. Such methods will be annotated with @Bean. 
    - The method annotated with this annotation works as bean ID and it creates and returns the actual bean.