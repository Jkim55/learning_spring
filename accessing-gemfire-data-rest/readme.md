# Create a web app that accesses Pivotal GemFire data thru a HATEOAS RESTful frontend 
Based on this[Spring Guide](https://spring.io/guides/gs/accessing-gemfire-data-rest/)

Spring Web application that let’s you create & retrieve Person objects stored in the Pivotal GemFire In-Memory Data Grid using Spring Data REST. 
Spring Data REST takes the features of Spring HATEOAS and Spring Data GemFire and combines them together automatically.

### Steps Taken to Create The Project
1. Create a new Gradle Project via IntelliJ
2. Create a new domain object to represent a person
    - Pivotal GemFire domain objects need an id; using AtomicLong to auto increment an id
3. Create a person repository to persist/access Person objects stored in Gemfire (see: PersonRepository.java)
    - This is an interface that by extending CRUDRepository will allow the applicaiton to perform various data access 
      operations (e.g. basic CRUD and simple queries) involving Person object
    - Spring Data GemFire will create an implementation of this interface automatically. 
    - Then, Spring Data REST will use the ```@RepositoryRestResource``` directs Spring MVC to create REST-ful endpoints at /people. 
    - ```@RepositoryRestResource``` isn't required for a Repository to be exported; used to change the export details, such as using /people instead of the default value of /persons
4. Make the application executable in an Application class
    - As before you can opt to package everything in a single, executable JAR file, driven by the Java main() method, which along the way, 
      you use Spring’s support for embedding the Tomcat servlet container as the HTTP runtime, instead of deploying to an external servlet container.
    - Wire up the repository
        - ```@EnableGemfireRepositories``` activates Spring Data GemFire Repositories. Spring Data GemFire will create a concrete 
          implementation of the PersonRepository interface and configure it to talk to an embedded instance of Pivotal GemFire.
        - implement a method to create the region and annotate it with ```@Bean("People")```
    

### Annotations
#### Person.java
1. @Data (from lombok)
    - @Data is a convenient shortcut annotation that bundles the features of @ToString, @EqualsAndHashCode, @Getter / @Setter and @RequiredArgsConstructor together
    - In other words, @Data generates all the boilerplate that is normally associated with simple POJOs (Plain Old Java Objects) and beans: 
      getters for all fields, setters for all non-final fields, and appropriate toString, equals and hashCode implementations that involve the fields of the class, and a constructor that initializes all final fields, as well as all non-final fields with no initializer that have been marked with @NonNull, in order to ensure the field is never null. 
2. @Region("People") 
    - specifies which Gemfire region the POJO is going to be stored in.
    - entities can be stored in GemFire Sub-Regions, as so: ```@Region("/Users/Admin")``` and also ```@Region("/Users/Guest")```    
3. @Id - can be used to annotate the property that shall be used as the Cache key
4. @PersistenceConstructor - helps disambiguating multiple potentially available constructors taking parameters and explicitly marking the one annotated as the one to be used to create entities. With none or only a single constructor you can omit the annotation.

#### PersonRepository.java
1. @RepositoryRestResource(collectionResourceRel = "people", path = "people")
    - directs Spring MVC to create REST-ful endpoints at /people
    - not required for a Repository to be exported; used to change the export details, such as using /people instead of the default value of /persons 
2. @Param("name") - gives a method parameter a concrete name and bind the name in the query

#### Application.java
1. @ClientCacheApplication(name = "DataGemFireRestApplication", logLevel = "error")
    - A GemFire cache containing 1 or more Regions is required to store all the data. To do so, use the app must use one of Spring Data GemFire’s configuration-based annotations
    - One of Spring Data GemFire’s convenient configuration-based annotations: @ClientCacheApplication, @PeerCacheApplication or `@CacheServerApplication
2. @EnableGemfireRepositories
    - Annotation to enable Gemfire repositories
    - Spring Data GemFire will create a concrete implementation of the PersonRepository interface & configure it to talk to an embedded instance of Pivotal GemFire.
3. @SuppressWarnings("unused")
    - allows Java programmers to disable compilation warnings for a certain part of a program (type, field, method, parameter, constructor, and local variable)
4. @Bean("People") 
    - tells Spring that a method annotated w/@Bean will return an object that should be registered as a bean in the Spring application context
    - aka annotated methods that define instantiation, configuration, and initialization logic for objects that are managed by the Spring IoC containera
    - the additional parameter tells Spring that the bean will be called People instead of Person???

