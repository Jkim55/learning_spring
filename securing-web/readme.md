# Securing a Web Application
Based on this[Spring Guide](https://spring.io/guides/gs/securing-web/)

Creating a simple web application with resources that are protected by Spring Security



### Steps Taken to Create The Project
1. Create a new Gradle Project via IntelliJ

2. Create thymeleaf templates that you'll need
    - home.html: Accessible to all users when authenticated
    - hello.html: Accessible to all users when authenticated
    - login.html: Redirected here if not autenticated. When user has been logged out or there is an error, this page displays different text
        - As configured, Spring Security provides a filter that intercepts that request and authenticates the user. 
            - If the user fails to authenticate, the page is redirected to "/login?error" and our page displays the appropriate error message
            - Upon successfully signing out, our application is sent to "/login?logout" and our page displays the appropriate success message.

3. Configure Spring MVC since the web app is based on Spring MVC & set up view controllers to expose the
se templates
    - Here’s a configuration class for configuring Spring MVC in the application

4. Set up Spring Security by configuring Spring Security in the application
    - Create WebSecurityConfig class and add Spring Security to the classpath with the @EnableWebSecurity
    - The configure(HttpSecurity) method defines which URL paths should be secured and which should not
    - The loginPage() method specifies the login page
    - configureGlobal(AuthenticationManagerBuilder) sets up an in-memory user store with a single user
     


### Annotations
#### WebSecurityConfig.java
- @Configuration
- @EnableWebSecurity: enables Spring Security’s web security support and provide the Spring MVC integration 
    - It also extends WebSecurityConfigurerAdapter and overrides a couple of its methods to set some specifics of the web security configuration.
    - Spring Boot automatically secures all HTTP endpoints with "basic" authentication; the security settings can be customized
    - The configure(HttpSecurity) method defines which URL paths should be secured and which should not
- @Autowired
- More on[Spring Security Architecture](https://spring.io/guides/topicals/spring-security-architecture/) - For more details on Spring Security
    
    
#### MvcConfig.java
- @Override

