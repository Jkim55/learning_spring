# Create a Server Application that Can Receive HTTP Multi-Part File Uploads
Based on this[Spring Guide](https://spring.io/guides/gs/uploading-files/)

A Spring Boot web application that accepts file uploads via a simple HTML interface.

### Steps Taken to Create The Project
1. Create a new Gradle Project via IntelliJ
2. Create a controller 
3. Set up a service for the controller to interact with a storage layer (e.g. a file system)
4. Creating a simple HTML template with ThymeLeaf that is composed of 3 parts:
    - An optional message at the top where Spring MVC writes a flash-scoped messages.
    - A form allowing the user to upload files
    - A list of files supplied from the backend
5. Add properties to properties settings (src/main/resources/application.properties)
6. Make the application executable in an Application class
        - main class
        - an init method


### Annotations
#### FileUploadController.java
@Controller
@Autowired
@GetMapping("/")
@ResponseBody
@PostMapping("/")
@ExceptionHandler(StorageFileNotFoundException.class)

#### Application.java
@EnableConfigurationProperties(StorageProperties.class)

