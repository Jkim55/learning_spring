# Create a Server Application that Can Receive HTTP Multi-Part File Uploads
Based on this[Spring Guide](https://spring.io/guides/gs/uploading-files/)

A Spring Boot web application that accepts file uploads via a simple HTML interface.

### Steps Taken to Create The Project
1. Create a new Gradle Project via IntelliJ

2. Create a file upload controller
    - Contorller wires up an instance of storage service
    - GET / looks up the current list of uploaded files from the StorageService and loads it into a Thymeleaf template. It calculates a link to the actual resource using MvcUriComponentsBuilder
    - GET /files/{filename} loads the resource if it exists, and sends it to the browser to download using a "Content-Disposition" response header
    - POST / is geared to handle a multi-part message file and give it to the StorageService for saving

3. Set up a StorageService 
    - a service is required for the controller to interact with a storage layer (e.g. a file system)
    - in this project a StorageService is created as an interface that isimplemented by the FileSystemStorageService
    
4. Creating a simple HTML template with ThymeLeaf that is composed of 3 parts:
    - An optional message at the top where Spring MVC writes a flash-scoped messages.
    - A form allowing the user to upload files
    - A list of files supplied from the backend
    
5. Add properties to properties settings (src/main/resources/application.properties)
    - Configure the file upload and set limits on the size of the files by tuning the auto-configured MultipartConfigElement with some property settings.                                
    - Set the following constraints:
        - spring.http.multipart.max-file-size is set to 128KB, meaning total file size cannot exceed 128KB.
        - spring.http.multipart.max-request-size is set to 128KB, meaning total request size for a multipart/form-data cannot exceed 128KB.

6. Make the application executable in an Application class
        - main class
        - an init method that adds a Boot CommandLineRunner which deletes and re-creates that folder at startup

### Annotations
#### FileUploadController.java
1. @Controller - used to mark any java class as a controller class.
2. @Autowired 
    - applied on fields, setter methods, and constructors
    - injects object dependency implicitly. 
    - When you use @Autowired on fields and pass the values for the fields using the property name, Spring will automatically assign the fields with the passed values.

3. @GetMapping("/")
    - Used for mapping HTTP GET requests onto specific handler methods. 
    - @GetMapping is a composed annotation that acts as a shortcut for @RequestMapping(method = RequestMethod.GET)
    
4. @ResponseBody
    - This annotation is used to annotate request handler methods. 
    - The @ResponseBody annotation is similar to the @RequestBody annotation. 
    - The @ResponseBody annotation indicates that the result type should be written straight in the response body in whatever format you specify like JSON or XML. Spring converts the returned object into a response body by using the HttpMessageConveter.

5. @PostMapping("/")
    - This annotation is used for mapping HTTP POST requests onto specific handler methods. 
    - @PostMapping is a composed annotation that acts as a shortcut for @RequestMapping(method = RequestMethod.POST)
    
6. @ExceptionHandler(StorageFileNotFoundException.class)
    - This annotation is used at method levels to handle exception at the controller level. 
    - The @ExceptionHandler annotation is used to define the class of exception it will catch. 
    - You can use this annotation on methods that should be invoked to handle an exception. 
    - The @ExceptionHandler values can be set to an array of Exception types. If an exception is thrown that matches one of the types in the list, then the method annotated with matching @ExceptionHandler will be invoked.

#### Application.java
1. @EnableConfigurationProperties(StorageProperties.class)
    - The @EnableConfigurationProperties annotation is also automatically applied to your project so that any existing bean annotated with @ConfigurationProperties will be configured from the Environment

#### StorageProperties.java
1. @ConfigurationProperties("storage")
    - Used to annotate a class, you can also use it on public @Bean methods. 
    - This can be particularly useful when you want to bind properties to third-party components that are outside of your control.
    - To configure a bean from the Environment properties, add @ConfigurationProperties to its bean registration
    
#### FileSystemStorageService.java
1. @Service 
    - Used on a class to mark a Java class that performs some service, such as execute business logic, perform calculations and call external APIs. 
    - This annotation is a specialized form of the @Component annotation intended to be used in the service layer.

2. @Override
    - informs the compiler that the element is meant to override an element declared in a superclass. 