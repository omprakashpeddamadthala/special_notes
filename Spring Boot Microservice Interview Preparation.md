# Spring Boot Microservice Interview Preparation 🚀

*A comprehensive guide with 100 questions, deep explanations, real-time use cases, and code examples*

---

## 📚 Table of Contents

1. [Spring Boot Fundamentals](#spring-boot-fundamentals) (Q1-Q25)
2. [Microservices Architecture](#microservices-architecture) (Q26-Q50)
3. [Spring Cloud and Service Discovery](#spring-cloud-and-service-discovery) (Q51-Q75)
4. [Advanced Topics and Best Practices](#advanced-topics-and-best-practices) (Q76-Q100)

---

## Spring Boot Fundamentals

### Q1: What is Spring Boot? 🤔

**Answer:** Spring Boot is an open-source, opinionated framework that simplifies the development of stand-alone, production-ready Spring applications with minimal configuration. 🚀

**Deep Explanation:** Spring Boot is built on top of the Spring Framework. It uses "convention over configuration" to automatically set up your application based on the dependencies you include. This means less boilerplate code and faster development. For example, if you add the `spring-boot-starter-web` dependency, Spring Boot automatically configures an embedded Tomcat server and Spring MVC for you. ✨

**Real-time Use Case:** Imagine building a new REST API for an e-commerce product catalog. With Spring Boot, you can have a basic API running in minutes, allowing you to focus on business logic instead of complex setup. 🛍️

**Code Example:**
```java
// src/main/java/com/example/productcatalog/ProductCatalogApplication.java
package com.example.productcatalog;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication
@RestController
public class ProductCatalogApplication {

    public static void main(String[] args) {
        SpringApplication.run(ProductCatalogApplication.class, args);
    }

    @GetMapping("/products")
    public String getProducts() {
        return "List of products";
    }
}
```

---

### Q2: What are Spring Boot Starters? ✨

**Answer:** Spring Boot Starters are pre-configured dependency descriptors that simplify your build configuration by providing all the necessary dependencies for a specific feature. 📦

**Deep Explanation:** Instead of manually adding multiple libraries and worrying about version compatibility, you can include a single starter. For example, `spring-boot-starter-data-jpa` includes everything you need for database access with JPA, including Hibernate and Spring Data. This reduces the chance of dependency conflicts. 🤝

**Real-time Use Case:** When building a microservice that needs to connect to a database, you can simply add `spring-boot-starter-data-jpa` to your `pom.xml`. This single dependency pulls in all the required libraries, making your data layer setup much easier. 💾

**Code Example (Maven `pom.xml`):**
```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```

---

### Q3: How does Spring Boot Auto-configuration work? 🪄

**Answer:** Spring Boot auto-configuration automatically configures your application based on the JARs on your classpath. It intelligently guesses how you want to configure Spring by looking at the dependencies you've added. 🤖

**Deep Explanation:** Auto-configuration works using `@Conditional` annotations. For example, `@ConditionalOnClass` checks if a specific class is present. If you have an H2 database JAR on your classpath, Spring Boot will automatically configure an in-memory H2 database for you. This reduces the need for manual configuration. ⚙️

**Real-time Use Case:** When you add `spring-boot-starter-web`, auto-configuration detects web-related JARs and automatically configures a `DispatcherServlet`, an embedded Tomcat server, and JSON message converters. You don't need to write any configuration for these basic web components. 🌐

**Code Example:**
```java
// Auto-configuration example - no manual configuration needed!
@SpringBootApplication
public class WebApplication {
    public static void main(String[] args) {
        SpringApplication.run(WebApplication.class, args);
        // Tomcat server starts automatically on port 8080
    }
}
```

---

### Q4: What are embedded servers in Spring Boot? 📦

**Answer:** Embedded servers in Spring Boot are web servers (like Tomcat, Jetty, or Undertow) that are packaged directly within your application's executable JAR file. This allows you to run your application as a stand-alone executable. 🏃‍♂️

**Deep Explanation:** Traditionally, Java web applications were deployed as WAR files to external servers. Spring Boot simplifies this by embedding the server. When you run the JAR, the embedded server starts up, and your application is deployed within it. This makes deployment much simpler. 🚀

**Real-time Use Case:** In a microservices architecture, each service can be packaged as a self-contained executable JAR with an embedded server. This allows for easy deployment to cloud environments and simplifies CI/CD pipelines. ☁️

**Code Example:**
```bash
# Build and run a Spring Boot application
mvn clean package
java -jar target/my-microservice-1.0.0.jar
# Server starts on http://localhost:8080
```

---

### Q5: What is the purpose of `@SpringBootApplication` annotation? 🎯

**Answer:** The `@SpringBootApplication` annotation is a convenience annotation that marks the main class of a Spring Boot application. It combines `@Configuration`, `@EnableAutoConfiguration`, and `@ComponentScan`. 🎬

**Deep Explanation:**
- `@Configuration`: Tags the class as a source of bean definitions.
- `@EnableAutoConfiguration`: Tells Spring Boot to start adding beans based on classpath settings.
- `@ComponentScan`: Tells Spring to scan for other components, configurations, and services.

This single annotation provides a quick and easy way to set up a basic Spring Boot application. 🏁

**Real-time Use Case:** When you create a new Spring Boot project, the main application class is automatically generated with `@SpringBootApplication`. This allows you to immediately start adding your business logic without spending time on initial Spring configuration. 🏎️

**Code Example:**
```java
@SpringBootApplication
public class ECommerceApplication {
    public static void main(String[] args) {
        SpringApplication.run(ECommerceApplication.class, args);
    }
}

// Equivalent to:
@Configuration
@EnableAutoConfiguration
@ComponentScan
public class ECommerceApplication {
    // ... same main method
}
```

---

### Q6: What is the difference between Spring Boot and Spring Framework? ↔️

**Answer:** Spring Framework is a comprehensive framework for building enterprise Java applications. Spring Boot is an extension of the Spring Framework that simplifies the development of stand-alone, production-ready Spring applications. 🛠️

**Deep Explanation:** Think of Spring Framework as the foundation and Spring Boot as a specialized tool built on top of it. Spring Boot provides auto-configuration, embedded servers, and opinionated defaults to make development faster and less cumbersome. You use Spring Boot *with* the Spring Framework. 🤝

**Real-time Use Case:** For a new microservice, Spring Boot is the preferred choice for rapid development. For a legacy Spring application with extensive XML configurations, you might be using just the Spring Framework. However, you could migrate parts of it to Spring Boot to leverage its benefits. 🔄

**Comparison Table:**
| Spring Framework | Spring Boot |
|------------------|-------------|
| Manual configuration | Auto-configuration |
| External server deployment | Embedded servers |
| XML/Java configuration | Convention over configuration |
| Complex setup | Rapid development |

---

### Q7: What is the purpose of `application.properties` or `application.yml`? ⚙️

**Answer:** `application.properties` (or `application.yml`) files in Spring Boot are used to externalize configuration for your application. They allow you to define properties like server port, database connection details, and other settings without modifying the code. 📝

**Deep Explanation:** Hardcoding values into your application is a bad practice. These files allow you to manage configurations for different environments (development, testing, production) easily. The YAML format (`.yml`) is often preferred for its readability and hierarchical structure. 🌳

**Real-time Use Case:** A microservice connecting to a database will have different database URLs for development and production. You can define these in `application.properties` and override them with environment-specific files (e.g., `application-prod.properties`) or environment variables. 🌍

**Code Example:**
```properties
# application.properties
server.port=8080
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.username=sa
spring.datasource.password=
logging.level.com.example=DEBUG
```

```yaml
# application.yml (equivalent)
server:
  port: 8080
spring:
  datasource:
    url: jdbc:h2:mem:testdb
    username: sa
    password: 
logging:
  level:
    com.example: DEBUG
```

---

### Q8: Explain Spring Profiles. 🎭

**Answer:** Spring Profiles provide a way to segregate parts of your application configuration and make them available only in certain environments. This allows you to have different configurations for development, testing, and production. 🔀

**Deep Explanation:** You can define profiles in `application.properties` or by naming your configuration files `application-{profile}.properties`. When a profile is active, only the beans and configurations for that profile are loaded. This ensures your application uses the correct settings for the specific environment. 🖼️

**Real-time Use Case:** In development, you might want to log emails to the console instead of sending them. In production, you'd want to use a real email service. Spring Profiles can manage this by activating different email service beans based on the active profile. ✉️

**Code Example:**
```properties
# application-dev.properties
spring.mail.host=localhost
logging.level.root=DEBUG
email.service.type=console

# application-prod.properties
spring.mail.host=smtp.gmail.com
spring.mail.username=${EMAIL_USERNAME}
spring.mail.password=${EMAIL_PASSWORD}
email.service.type=smtp
```

```java
@Component
@Profile("dev")
public class ConsoleEmailService implements EmailService {
    public void sendEmail(String to, String subject, String body) {
        System.out.println("Email to: " + to + ", Subject: " + subject);
    }
}

@Component
@Profile("prod")
public class SmtpEmailService implements EmailService {
    // Real SMTP implementation
}
```

---

### Q9: What is Dependency Injection (DI) in Spring? 💉

**Answer:** Dependency Injection (DI) is a design pattern where the dependencies of an object are provided by an external source (the Spring IoC container) rather than the object creating them itself. This promotes loose coupling. 🔗

**Deep Explanation:** Instead of a `UserService` creating a `UserRepository` directly, it simply declares that it needs one. The Spring container then provides (injects) an instance of `UserRepository` into `UserService`. This can be done through constructor injection (preferred), setter injection, or field injection. 🏗️

**Real-time Use Case:** A `ProductService` needs a `ProductRepository` and a `NotificationService`. With DI, you simply declare these as dependencies in the `ProductService` constructor, and Spring injects them. This makes the `ProductService` easier to test and maintain. 🧪

**Code Example:**
```java
@Service
public class ProductService {
    private final ProductRepository productRepository;
    private final NotificationService notificationService;
    
    // Constructor injection (recommended)
    public ProductService(ProductRepository productRepository, 
                         NotificationService notificationService) {
        this.productRepository = productRepository;
        this.notificationService = notificationService;
    }
    
    public Product createProduct(Product product) {
        Product savedProduct = productRepository.save(product);
        notificationService.notifyProductCreated(savedProduct);
        return savedProduct;
    }
}
```

---

### Q10: What is Spring Boot Actuator? 📊

**Answer:** Spring Boot Actuator provides production-ready features to help you monitor and manage your application. It exposes operational information like health, metrics, and environment properties through HTTP endpoints. 🩺

**Deep Explanation:** By adding the `spring-boot-starter-actuator` dependency, your application automatically exposes several endpoints, such as `/actuator/health`, `/actuator/info`, and `/actuator/metrics`. These are invaluable for monitoring, troubleshooting, and understanding the behavior of your microservices. 📈

**Real-time Use Case:** If a microservice becomes unresponsive, you can check `/actuator/health` to see if the database connection is down, or `/actuator/metrics` to identify a spike in error rates. This allows for quick diagnosis and resolution of issues. 🚑

**Code Example:**
```xml
<!-- Add to pom.xml -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

```properties
# application.properties
management.endpoints.web.exposure.include=health,info,metrics
management.endpoint.health.show-details=always
```

```bash
# Available endpoints
curl http://localhost:8080/actuator/health
curl http://localhost:8080/actuator/metrics
curl http://localhost:8080/actuator/info
```

---

### Q11: What is the difference between `@Component`, `@Service`, `@Repository`, and `@Controller`? 🧩

**Answer:** These are stereotype annotations to mark classes as Spring-managed components. `@Component` is generic, while `@Service`, `@Repository`, and `@Controller` are specialized for the service, persistence, and presentation layers, respectively. 🏷️

**Deep Explanation:**
- `@Component`: Generic stereotype for any Spring-managed component.
- `@Service`: Indicates a class in the service layer (business logic).
- `@Repository`: Indicates a class in the persistence layer (data access). It also enables exception translation.
- `@Controller`: Indicates a class in the presentation layer (web request handler).

Using these specialized annotations provides better semantic clarity and can enable specific features. 🏛️

**Code Example:**
```java
@Component
public class UtilityHelper {
    // Generic utility component
}

@Service
public class UserService {
    // Business logic for user operations
}

@Repository
public class UserRepository {
    // Data access operations
}

@Controller
public class UserController {
    // Web request handling
}

@RestController // Combines @Controller + @ResponseBody
public class UserRestController {
    // REST API endpoints
}
```

---

### Q12: What is `@Autowired` annotation? 🔗

**Answer:** The `@Autowired` annotation in Spring is used for automatic dependency injection. It tells the Spring container to automatically inject an instance of a dependency into a bean. 🤝

**Deep Explanation:** When Spring encounters `@Autowired`, it searches its application context for a matching bean. While it can be used on fields, setters, or constructors, **constructor injection is recommended** as it ensures the bean is fully initialized with its required dependencies. 💉

**Real-time Use Case:** A `UserService` needs a `UserRepository`. By annotating the `UserRepository` in the `UserService` constructor with `@Autowired`, Spring automatically provides the correct `UserRepository` instance. 🛠️

**Code Example:**
```java
@Service
public class UserService {
    private final UserRepository userRepository;
    
    // Constructor injection (recommended)
    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
    
    // Field injection (not recommended)
    // @Autowired
    // private UserRepository userRepository;
    
    // Setter injection
    // @Autowired
    // public void setUserRepository(UserRepository userRepository) {
    //     this.userRepository = userRepository;
    // }
}
```

---

### Q13: What is `@Bean` annotation? 🌿

**Answer:** The `@Bean` annotation is used to declare a method that instantiates, configures, and initializes a new object to be managed by the Spring IoC container. It is typically used within a `@Configuration` class. 🏭

**Deep Explanation:** While stereotype annotations automatically detect classes, `@Bean` provides a way to explicitly define a single bean. This is useful for configuring third-party classes or when you need fine-grained control over bean creation. 🎨

**Real-time Use Case:** A common use case is configuring a `RestTemplate` for making HTTP calls or setting up a `DataSource` for database connectivity. These are often third-party classes that need specific configuration. 🌐

**Code Example:**
```java
@Configuration
public class AppConfig {
    
    @Bean
    public RestTemplate restTemplate() {
        RestTemplate restTemplate = new RestTemplate();
        restTemplate.setRequestFactory(new HttpComponentsClientHttpRequestFactory());
        return restTemplate;
    }
    
    @Bean
    @Primary
    public DataSource primaryDataSource() {
        HikariDataSource dataSource = new HikariDataSource();
        dataSource.setJdbcUrl("jdbc:postgresql://localhost:5432/primary");
        dataSource.setUsername("user");
        dataSource.setPassword("password");
        return dataSource;
    }
}
```

---

### Q14: What is the purpose of `@RestController` and `@RequestMapping`? 🌐

**Answer:** `@RestController` combines `@Controller` and `@ResponseBody`, marking a class as a controller where every method returns a domain object serialized into JSON or XML. `@RequestMapping` maps web requests to handler methods. 🗺️

**Deep Explanation:** `@RestController` is ideal for building REST APIs. `@RequestMapping` can be applied at the class level (for a base URI) and method level. More specific annotations like `@GetMapping`, `@PostMapping`, etc., are preferred for clarity. 🚦

**Real-time Use Case:** Building a RESTful API for an e-commerce application. `@RestController` defines the endpoints, and `@RequestMapping` (or its specialized versions) ensures the correct methods are invoked for each incoming request. 🛒

**Code Example:**
```java
@RestController
@RequestMapping("/api/v1/products")
public class ProductController {
    
    private final ProductService productService;
    
    public ProductController(ProductService productService) {
        this.productService = productService;
    }
    
    @GetMapping
    public List<Product> getAllProducts() {
        return productService.findAll();
    }
    
    @GetMapping("/{id}")
    public Product getProductById(@PathVariable Long id) {
        return productService.findById(id);
    }
    
    @PostMapping
    public Product createProduct(@RequestBody Product product) {
        return productService.save(product);
    }
    
    @PutMapping("/{id}")
    public Product updateProduct(@PathVariable Long id, @RequestBody Product product) {
        return productService.update(id, product);
    }
    
    @DeleteMapping("/{id}")
    public void deleteProduct(@PathVariable Long id) {
        productService.delete(id);
    }
}
```

---

### Q15: What is `@Value` annotation? 📝

**Answer:** The `@Value` annotation is used to inject values from properties files, environment variables, or other configuration sources directly into fields or method parameters of Spring-managed beans. 💵

**Deep Explanation:** `@Value` provides a convenient way to access externalized properties within your Java code. The syntax is `${property.name}`, and you can provide a default value with `${property.name:defaultValue}`. ⚙️

**Real-time Use Case:** A microservice needs the URL of an external service. Instead of hardcoding it, you define it in `application.properties` and inject it using `@Value`. This makes your application more adaptable. 🌍

**Code Example:**
```java
@Service
public class PaymentService {
    
    @Value("${payment.gateway.url}")
    private String paymentGatewayUrl;
    
    @Value("${payment.gateway.timeout:5000}")
    private int timeout;
    
    @Value("${payment.gateway.api-key}")
    private String apiKey;
    
    public PaymentResponse processPayment(PaymentRequest request) {
        // Use the injected values
        RestTemplate restTemplate = new RestTemplate();
        // Configure timeout, headers with apiKey, etc.
        return restTemplate.postForObject(paymentGatewayUrl + "/charge", 
                                         request, PaymentResponse.class);
    }
}
```

```properties
# application.properties
payment.gateway.url=https://api.stripe.com/v1
payment.gateway.timeout=10000
payment.gateway.api-key=${STRIPE_API_KEY}
```

---

### Q16: How do you handle exceptions in Spring Boot REST APIs? 🚨

**Answer:** Exceptions are handled gracefully using `@ControllerAdvice` (or `@RestControllerAdvice`) combined with `@ExceptionHandler` annotations. This allows for centralized exception handling. 🛡️

**Deep Explanation:** `@ControllerAdvice` marks a class as a global exception handler. `@ExceptionHandler` specifies which exception a method should handle. Within these methods, you typically return a `ResponseEntity` to control the HTTP status code and response body. 🚑

**Real-time Use Case:** If a client requests a product by an ID that doesn't exist, you can throw a custom `ProductNotFoundException`. A global exception handler catches this and returns a `404 Not Found` status with a clear message. 🚫

**Code Example:**
```java
// Custom exception
public class ProductNotFoundException extends RuntimeException {
    public ProductNotFoundException(Long id) {
        super("Product not found with id: " + id);
    }
}

// Global exception handler
@RestControllerAdvice
public class GlobalExceptionHandler {
    
    @ExceptionHandler(ProductNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleProductNotFound(ProductNotFoundException ex) {
        ErrorResponse error = new ErrorResponse(
            "PRODUCT_NOT_FOUND", 
            ex.getMessage(),
            System.currentTimeMillis()
        );
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
    }
    
    @ExceptionHandler(ValidationException.class)
    public ResponseEntity<ErrorResponse> handleValidation(ValidationException ex) {
        ErrorResponse error = new ErrorResponse(
            "VALIDATION_ERROR", 
            ex.getMessage(),
            System.currentTimeMillis()
        );
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(error);
    }
    
    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGeneral(Exception ex) {
        ErrorResponse error = new ErrorResponse(
            "INTERNAL_ERROR", 
            "An unexpected error occurred",
            System.currentTimeMillis()
        );
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(error);
    }
}

// Error response DTO
public class ErrorResponse {
    private String code;
    private String message;
    private long timestamp;
    
    // constructors, getters, setters
}
```

---

### Q17: How do you perform unit testing in Spring Boot? 🧪

**Answer:** Unit testing in Spring Boot focuses on testing individual components in isolation, without the Spring context. This typically involves using JUnit 5 and mocking dependencies with Mockito. 🔬

**Deep Explanation:** Unit tests are the smallest and fastest tests. By mocking dependencies, you ensure that failures in your unit tests are directly attributable to the code under test, rather than issues in its dependencies. This makes debugging much easier. 🎯

**Real-time Use Case:** When unit testing a `ProductService`, you mock the `ProductRepository` to avoid hitting a real database. This allows you to test the `ProductService`'s business logic in isolation. 👨‍🔬

**Code Example:**
```java
@ExtendWith(MockitoExtension.class)
class ProductServiceTest {
    
    @Mock
    private ProductRepository productRepository;
    
    @Mock
    private NotificationService notificationService;
    
    @InjectMocks
    private ProductService productService;
    
    @Test
    void shouldCreateProductSuccessfully() {
        // Given
        Product inputProduct = new Product("Test Product", 99.99);
        Product savedProduct = new Product(1L, "Test Product", 99.99);
        
        when(productRepository.save(inputProduct)).thenReturn(savedProduct);
        
        // When
        Product result = productService.createProduct(inputProduct);
        
        // Then
        assertThat(result.getId()).isEqualTo(1L);
        assertThat(result.getName()).isEqualTo("Test Product");
        verify(productRepository).save(inputProduct);
        verify(notificationService).notifyProductCreated(savedProduct);
    }
    
    @Test
    void shouldThrowExceptionWhenProductNotFound() {
        // Given
        Long productId = 999L;
        when(productRepository.findById(productId)).thenReturn(Optional.empty());
        
        // When & Then
        assertThatThrownBy(() -> productService.findById(productId))
            .isInstanceOf(ProductNotFoundException.class)
            .hasMessage("Product not found with id: 999");
    }
}
```

---

### Q18: How do you perform integration testing in Spring Boot? 🤝

**Answer:** Integration testing involves testing the interaction between different components of your application, often including the Spring application context. Spring Boot provides `@SpringBootTest` and `@WebMvcTest` to facilitate these tests. 🏗️

**Deep Explanation:** Integration tests verify that different parts of your system work together as expected. `@SpringBootTest` loads the full Spring Boot application, while `@WebMvcTest` focuses on the web layer, and `@DataJpaTest` on the persistence layer. 🔗

**Real-time Use Case:** An integration test for a REST endpoint would involve starting the Spring application context, making an actual HTTP request, and verifying that the controller, service, and repository interact correctly. 🚀

**Code Example:**
```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@TestPropertySource(locations = "classpath:application-test.properties")
class ProductControllerIntegrationTest {
    
    @Autowired
    private TestRestTemplate restTemplate;
    
    @Autowired
    private ProductRepository productRepository;
    
    @Test
    void shouldCreateAndRetrieveProduct() {
        // Given
        Product product = new Product("Integration Test Product", 149.99);
        
        // When - Create product
        ResponseEntity<Product> createResponse = restTemplate.postForEntity(
            "/api/v1/products", product, Product.class);
        
        // Then - Verify creation
        assertThat(createResponse.getStatusCode()).isEqualTo(HttpStatus.CREATED);
        Product createdProduct = createResponse.getBody();
        assertThat(createdProduct.getId()).isNotNull();
        
        // When - Retrieve product
        ResponseEntity<Product> getResponse = restTemplate.getForEntity(
            "/api/v1/products/" + createdProduct.getId(), Product.class);
        
        // Then - Verify retrieval
        assertThat(getResponse.getStatusCode()).isEqualTo(HttpStatus.OK);
        Product retrievedProduct = getResponse.getBody();
        assertThat(retrievedProduct.getName()).isEqualTo("Integration Test Product");
        assertThat(retrievedProduct.getPrice()).isEqualTo(149.99);
    }
}

// Web layer testing
@WebMvcTest(ProductController.class)
class ProductControllerWebTest {
    
    @Autowired
    private MockMvc mockMvc;
    
    @MockBean
    private ProductService productService;
    
    @Test
    void shouldReturnProductWhenFound() throws Exception {
        // Given
        Product product = new Product(1L, "Test Product", 99.99);
        when(productService.findById(1L)).thenReturn(product);
        
        // When & Then
        mockMvc.perform(get("/api/v1/products/1"))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.id").value(1))
            .andExpect(jsonPath("$.name").value("Test Product"))
            .andExpect(jsonPath("$.price").value(99.99));
    }
}
```

---

### Q19: What is Spring Data JPA? 💾

**Answer:** Spring Data JPA simplifies data access layers by providing a high-level abstraction over JPA and Hibernate. It allows developers to implement repository interfaces by simply defining method names, reducing boilerplate code. 🗂️

**Deep Explanation:** You define interfaces that extend `JpaRepository`. Spring Data JPA then automatically generates the implementation for these interfaces at runtime. You can also define query methods by following a naming convention (e.g., `findByLastName`). ✍️

**Real-time Use Case:** For a `Product` entity, you can define a `ProductRepository` interface. Spring Data JPA will provide all CRUD operations automatically, saving you from writing boilerplate data access code. 🛒

**Code Example:**
```java
// Entity
@Entity
@Table(name = "products")
public class Product {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String name;
    
    @Column(nullable = false)
    private BigDecimal price;
    
    @Column
    private String category;
    
    // constructors, getters, setters
}

// Repository interface
@Repository
public interface ProductRepository extends JpaRepository<Product, Long> {
    
    // Query methods by naming convention
    List<Product> findByCategory(String category);
    List<Product> findByNameContainingIgnoreCase(String name);
    List<Product> findByPriceBetween(BigDecimal minPrice, BigDecimal maxPrice);
    
    // Custom query with @Query
    @Query("SELECT p FROM Product p WHERE p.price > :price ORDER BY p.price DESC")
    List<Product> findExpensiveProducts(@Param("price") BigDecimal price);
    
    // Native query
    @Query(value = "SELECT * FROM products WHERE category = ?1 LIMIT ?2", 
           nativeQuery = true)
    List<Product> findTopProductsByCategory(String category, int limit);
}

// Service using the repository
@Service
public class ProductService {
    private final ProductRepository productRepository;
    
    public ProductService(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }
    
    public List<Product> findProductsByCategory(String category) {
        return productRepository.findByCategory(category);
    }
    
    public List<Product> searchProducts(String searchTerm) {
        return productRepository.findByNameContainingIgnoreCase(searchTerm);
    }
}
```

---

### Q20: What is Spring Boot CLI? 💻

**Answer:** Spring Boot CLI (Command Line Interface) is a command-line tool that allows you to quickly prototype Spring applications using Groovy scripts. It enables rapid development by leveraging auto-configuration and starters. ⚡

**Deep Explanation:** The CLI uses Groovy, so you don't need to write traditional Java boilerplate. It automatically detects common dependencies and adds the necessary starters. This is great for quick prototypes or small utilities. 🏃‍♀️

**Real-time Use Case:** You need to quickly create a small REST endpoint to test a concept. Instead of setting up a full Maven/Gradle project, you can write a few lines of Groovy code and run it directly with the Spring Boot CLI. 🚀

**Code Example:**
```groovy
// hello.groovy
@RestController
class HelloController {
    @RequestMapping("/")
    String hello() {
        "Hello World from Spring Boot CLI!"
    }
}
```

```bash
# Install Spring Boot CLI
sdk install springboot

# Run the Groovy script
spring run hello.groovy

# Access the endpoint
curl http://localhost:8080/
```

---

### Q21: What is the difference between `@RequestMapping` and `@GetMapping`/`@PostMapping` etc.? 🔄

**Answer:** `@RequestMapping` is a general-purpose mapping annotation. `@GetMapping`, `@PostMapping`, etc., are specialized shortcuts for `@RequestMapping` with a specific HTTP method, improving readability. 🚦

**Deep Explanation:** While `@RequestMapping` is versatile, the specialized annotations make the intent of your API endpoints much clearer. They are the preferred way to define REST endpoints in modern Spring applications. 🗺️

**Real-time Use Case:** For a user management API, you would use `@PostMapping("/users")` for creating a user, `@GetMapping("/users/{id}")` for retrieving one, and so on. This is more intuitive than using `@RequestMapping` for everything. 👥

**Code Example:**
```java
@RestController
@RequestMapping("/api/v1/users")
public class UserController {
    
    // Old way - using @RequestMapping
    @RequestMapping(value = "/old-style", method = RequestMethod.GET)
    public List<User> getAllUsersOldStyle() {
        return userService.findAll();
    }
    
    // Modern way - using specialized annotations
    @GetMapping
    public List<User> getAllUsers() {
        return userService.findAll();
    }
    
    @GetMapping("/{id}")
    public User getUserById(@PathVariable Long id) {
        return userService.findById(id);
    }
    
    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.save(user);
    }
    
    @PutMapping("/{id}")
    public User updateUser(@PathVariable Long id, @RequestBody User user) {
        return userService.update(id, user);
    }
    
    @DeleteMapping("/{id}")
    public void deleteUser(@PathVariable Long id) {
        userService.delete(id);
    }
    
    @PatchMapping("/{id}")
    public User partialUpdateUser(@PathVariable Long id, @RequestBody Map<String, Object> updates) {
        return userService.partialUpdate(id, updates);
    }
}
```

---

### Q22: What is the purpose of `@PathVariable` and `@RequestParam`? 🧩

**Answer:** `@PathVariable` extracts values from the URI path itself (e.g., `/users/123`), while `@RequestParam` extracts values from the query parameters in the URL (e.g., `/products?category=electronics`). 📌

**Deep Explanation:** Use `@PathVariable` when the value is part of the resource's identifier. Use `@RequestParam` for filtering, sorting, or providing optional data. 🎛️

**Real-time Use Case:** For a blog API, `/posts/{id}` would use `@PathVariable` for the post ID. `/posts?author=JohnDoe&category=tech` would use `@RequestParam` for the author name and category. ✍️

**Code Example:**
```java
@RestController
@RequestMapping("/api/v1")
public class BlogController {
    
    // @PathVariable examples
    @GetMapping("/posts/{id}")
    public Post getPostById(@PathVariable Long id) {
        return postService.findById(id);
    }
    
    @GetMapping("/users/{userId}/posts/{postId}")
    public Post getUserPost(@PathVariable Long userId, @PathVariable Long postId) {
        return postService.findByUserIdAndPostId(userId, postId);
    }
    
    // @RequestParam examples
    @GetMapping("/posts")
    public List<Post> getPosts(
            @RequestParam(required = false) String author,
            @RequestParam(required = false) String category,
            @RequestParam(defaultValue = "0") int page,
            @RequestParam(defaultValue = "10") int size,
            @RequestParam(defaultValue = "createdDate") String sortBy) {
        
        return postService.findPosts(author, category, page, size, sortBy);
    }
    
    // Mixed usage
    @GetMapping("/users/{userId}/posts")
    public List<Post> getUserPosts(
            @PathVariable Long userId,
            @RequestParam(required = false) String status,
            @RequestParam(defaultValue = "0") int page) {
        
        return postService.findUserPosts(userId, status, page);
    }
}
```

---

### Q23: What is the purpose of `@RequestBody` and `@ResponseBody`? 🔄

**Answer:** `@RequestBody` binds the HTTP request body (e.g., JSON) to a method parameter. `@ResponseBody` binds the return value of a method to the HTTP response body, converting it to JSON or XML. ↔️

**Deep Explanation:** These annotations simplify the process of converting between Java objects and HTTP message bodies. `@RestController` is a convenience annotation that includes `@ResponseBody` for all methods. 📦

**Real-time Use Case:** When creating a new product, the client sends a `POST` request with a JSON payload. The controller method uses `@RequestBody` to convert this JSON into a `Product` object. The method then returns the created `Product`, which is converted back to JSON for the response. 🛍️

**Code Example:**
```java
@RestController
@RequestMapping("/api/v1/products")
public class ProductController {
    
    // @RequestBody example
    @PostMapping
    public ResponseEntity<Product> createProduct(@RequestBody Product product) {
        // The JSON in request body is automatically converted to Product object
        Product savedProduct = productService.save(product);
        // The returned Product is automatically converted to JSON response
        return ResponseEntity.status(HttpStatus.CREATED).body(savedProduct);
    }
    
    @PutMapping("/{id}")
    public Product updateProduct(@PathVariable Long id, @RequestBody Product product) {
        return productService.update(id, product);
    }
    
    // Complex request body
    @PostMapping("/bulk")
    public List<Product> createProducts(@RequestBody List<Product> products) {
        return productService.saveAll(products);
    }
    
    // Custom DTO for request body
    @PostMapping("/search")
    public List<Product> searchProducts(@RequestBody ProductSearchRequest searchRequest) {
        return productService.search(searchRequest);
    }
}

// DTO for complex request
public class ProductSearchRequest {
    private String name;
    private String category;
    private BigDecimal minPrice;
    private BigDecimal maxPrice;
    private List<String> tags;
    
    // getters and setters
}

// Example JSON request body for POST /api/v1/products
/*
{
    "name": "Wireless Headphones",
    "price": 199.99,
    "category": "Electronics",
    "description": "High-quality wireless headphones with noise cancellation"
}
*/
```

---

### Q24: What is the purpose of `@ConfigurationProperties`? ⚙️

**Answer:** `@ConfigurationProperties` provides a type-safe way to bind external configuration properties to Java objects. This allows you to group related properties and validate them. 묶음

**Deep Explanation:** Instead of injecting single properties with `@Value`, you can create a POJO with fields that match your configuration properties. Spring Boot automatically binds the values. This is cleaner and less error-prone. 🛡️

**Real-time Use Case:** For a payment gateway integration, you can create a `PaymentGatewayProperties` class to hold the API key, secret, URL, and timeout settings. This makes the configuration easier to manage. 💳

**Code Example:**
```java
// Configuration properties class
@ConfigurationProperties(prefix = "payment.gateway")
@Component
public class PaymentGatewayProperties {
    private String url;
    private String apiKey;
    private String secret;
    private int timeoutMs = 5000;
    private boolean enabled = true;
    private List<String> supportedCurrencies;
    private Map<String, String> headers;
    
    // getters and setters
    public String getUrl() { return url; }
    public void setUrl(String url) { this.url = url; }
    
    public String getApiKey() { return apiKey; }
    public void setApiKey(String apiKey) { this.apiKey = apiKey; }
    
    // ... other getters and setters
}

// Service using the properties
@Service
public class PaymentService {
    private final PaymentGatewayProperties properties;
    
    public PaymentService(PaymentGatewayProperties properties) {
        this.properties = properties;
    }
    
    public PaymentResponse processPayment(PaymentRequest request) {
        if (!properties.isEnabled()) {
            throw new PaymentException("Payment gateway is disabled");
        }
        
        // Use properties.getUrl(), properties.getApiKey(), etc.
        RestTemplate restTemplate = new RestTemplate();
        // Configure with timeout, headers, etc.
        return restTemplate.postForObject(properties.getUrl() + "/charge", 
                                         request, PaymentResponse.class);
    }
}
```

```properties
# application.properties
payment.gateway.url=https://api.stripe.com/v1
payment.gateway.api-key=${STRIPE_API_KEY}
payment.gateway.secret=${STRIPE_SECRET}
payment.gateway.timeout-ms=10000
payment.gateway.enabled=true
payment.gateway.supported-currencies=USD,EUR,GBP
payment.gateway.headers.Content-Type=application/json
payment.gateway.headers.User-Agent=MyApp/1.0
```

---

### Q25: What is Spring Boot DevTools? 🛠️

**Answer:** Spring Boot DevTools is a set of tools that improves the development experience by providing features like automatic application restarts, live reload, and automatic browser refresh. 🔄

**Deep Explanation:** DevTools significantly speeds up the development cycle. When you save a change, it automatically restarts the application or refreshes the browser, providing instant feedback. It's intended for development only. 🚀

**Real-time Use Case:** When developing a web application, every time you change a line of Java code or modify an HTML template, DevTools automatically handles the restart and refresh, saving you from manual steps. 🏎️

**Code Example:**
```xml
<!-- Add to pom.xml -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <scope>runtime</scope>
    <optional>true</optional>
</dependency>
```

```properties
# application.properties (DevTools configuration)
spring.devtools.restart.enabled=true
spring.devtools.livereload.enabled=true
spring.devtools.restart.additional-paths=src/main/resources
spring.devtools.restart.exclude=static/**,public/**
```

**Features:**
- **Automatic Restart**: Restarts the application when classpath files change
- **Live Reload**: Automatically refreshes the browser when resources change
- **Property Defaults**: Provides sensible defaults for development (e.g., disables template caching)
- **Remote Applications**: Can connect to remote applications for debugging

---

## Microservices Architecture

### Q26: What are Microservices? 🏗️

**Answer:** Microservices are an architectural style that structures an application as a collection of small, autonomous services, modeled around a business domain. Each service is independently deployable, scalable, and maintainable. 🏢

**Deep Explanation:** Microservices break down a large monolithic application into smaller, independent services. Each service focuses on a single business capability, owns its data, and communicates with others through lightweight mechanisms like REST APIs. This promotes agility, resilience, and scalability. 🚀

**Real-time Use Case:** An e-commerce platform like Amazon is composed of many microservices. There's a `Product Catalog Service`, a `Shopping Cart Service`, an `Order Processing Service`, and so on. Each can be developed, deployed, and scaled independently. 🛒

**Code Example:**
```java
// Product Service
@RestController
@RequestMapping("/api/products")
public class ProductController {
    @GetMapping("/{id}")
    public Product getProduct(@PathVariable Long id) {
        return productService.findById(id);
    }
}

// Order Service
@RestController
@RequestMapping("/api/orders")
public class OrderController {
    @PostMapping
    public Order createOrder(@RequestBody OrderRequest request) {
        // Calls Product Service to validate products
        // Calls Inventory Service to check availability
        // Calls Payment Service to process payment
        return orderService.createOrder(request);
    }
}

// Each service has its own database and deployment
```

---

### Q27: What are the advantages of Microservices? ✅

**Answer:** Microservices offer independent deployability, improved scalability, technological diversity, enhanced fault isolation, and better organizational alignment with small, autonomous teams. 👍

**Deep Explanation:**
- **Independent Deployability:** Deploy services without affecting others.
- **Improved Scalability:** Scale individual services based on load.
- **Technological Diversity:** Use the best technology for each service.
- **Fault Isolation:** Failure in one service doesn't bring down the whole system.
- **Easier Maintenance:** Smaller codebases are easier to understand.

These advantages lead to more robust, flexible, and efficient systems. 🌟

**Real-time Use Case:**
- **Netflix**: Can deploy hundreds of times per day because each service is independent
- **Amazon**: Different teams can choose different technologies (Java, Python, Go) for their services
- **Uber**: Can scale the ride-matching service independently from the payment service during peak hours

---

### Q28: What are the disadvantages of Microservices? ❌

**Answer:** Microservices introduce complexities like increased operational overhead, distributed system challenges (data consistency, latency), higher development complexity, and the need for robust monitoring. 🤯

**Deep Explanation:**
- **Operational Complexity:** Requires sophisticated infrastructure for deployment and monitoring.
- **Distributed System Challenges:** Network communication, data consistency, and distributed tracing are hard.
- **Testing Complexity:** Integration testing becomes more complex.
- **Security Challenges:** Securing communication between services adds complexity.

Microservices are best for complex, large-scale applications where the benefits outweigh these challenges. 🚧

**Real-time Use Case:**
- A simple blog application might be over-engineered as microservices
- Small teams might struggle with the operational overhead
- Network latency between services can impact performance

---

### Q29: What are the key principles of Microservices? 🌟

**Answer:** Key principles include single responsibility, independent deployment, decentralized data management, smart endpoints and dumb pipes, and fault tolerance. 📜

**Deep Explanation:**
- **Single Responsibility:** Each service does one thing well.
- **Independent Deployment:** Services can be deployed independently.
- **Decentralized Data Management:** Each service owns its data.
- **Smart Endpoints and Dumb Pipes:** Services expose rich APIs and communicate via simple protocols.
- **Fault Tolerance:** Services are designed to handle failures gracefully.

Following these principles is crucial for building effective microservice architectures. 💡

---

### Q30: What is a Monolithic Architecture? 🏛️

**Answer:** A monolithic architecture is a traditional approach where all components of an application are tightly coupled and packaged together as a single, indivisible unit. 🧱

**Deep Explanation:** In a monolith, all functionalities are in one codebase and deployed as one large application. While simple to start, monoliths become difficult to manage, scale, and update as they grow. 🐘

**Real-time Use Case:** Many legacy enterprise applications are monoliths. A banking application might have all its modules (account management, loans, transactions) bundled into one large application. 🏦

**Comparison:**
| Monolithic | Microservices |
|------------|---------------|
| Single deployment unit | Multiple deployment units |
| Shared database | Database per service |
| Technology lock-in | Technology diversity |
| Simple to start | Complex to start |
| Difficult to scale parts | Easy to scale parts |

---

### Q31: How do microservices communicate with each other? 📞

**Answer:** Microservices communicate through lightweight mechanisms, most commonly synchronous HTTP/REST APIs for request-response interactions, and asynchronous message queues or event streams for event-driven communication. 💬

**Deep Explanation:**
- **Synchronous (HTTP/REST, gRPC):** The client sends a request and waits for a response. Good for immediate feedback.
- **Asynchronous (Message Queues like Kafka, RabbitMQ):** The sender publishes a message, and the receiver consumes it independently. Good for decoupling services and long-running processes.

Often, a hybrid approach is used. 🔄

**Code Example:**
```java
// Synchronous communication with RestTemplate
@Service
public class OrderService {
    private final RestTemplate restTemplate;
    
    public Order createOrder(OrderRequest request) {
        // Synchronous call to Product Service
        Product product = restTemplate.getForObject(
            "http://product-service/api/products/" + request.getProductId(), 
            Product.class);
        
        // Synchronous call to Inventory Service
        boolean available = restTemplate.getForObject(
            "http://inventory-service/api/inventory/check/" + request.getProductId(), 
            Boolean.class);
        
        if (available) {
            return processOrder(request, product);
        } else {
            throw new ProductNotAvailableException();
        }
    }
}

// Asynchronous communication with messaging
@Component
public class OrderEventPublisher {
    private final KafkaTemplate<String, Object> kafkaTemplate;
    
    public void publishOrderCreated(Order order) {
        OrderCreatedEvent event = new OrderCreatedEvent(order.getId(), 
                                                        order.getCustomerId(), 
                                                        order.getTotal());
        kafkaTemplate.send("order-events", event);
    }
}

@KafkaListener(topics = "order-events")
@Component
public class InventoryEventHandler {
    public void handleOrderCreated(OrderCreatedEvent event) {
        // Update inventory asynchronously
        inventoryService.reserveItems(event.getOrderId());
    }
}
```

---

### Q32: Explain synchronous communication in microservices. ↔️

**Answer:** Synchronous communication involves a client service sending a request to a server service and then waiting for an immediate response. This is typically done with protocols like HTTP/REST or gRPC. ⏳

**Deep Explanation:** It's like a phone call: you wait for the other person to answer and reply. This pattern introduces temporal coupling (both services must be available) and requires robust error handling for network issues and timeouts. 📞

**Real-time Use Case:** When a user logs in, the `Frontend Service` makes a synchronous call to the `Authentication Service` to verify credentials. The frontend must wait for the response before proceeding. 🔐

**Code Example:**
```java
// Using OpenFeign for synchronous communication
@FeignClient(name = "user-service", url = "${user-service.url}")
public interface UserServiceClient {
    @GetMapping("/api/users/{id}")
    User getUserById(@PathVariable Long id);
    
    @PostMapping("/api/users/{id}/validate")
    boolean validateUser(@PathVariable Long id, @RequestBody ValidationRequest request);
}

@Service
public class OrderService {
    private final UserServiceClient userServiceClient;
    
    public Order createOrder(OrderRequest request) {
        // Synchronous call - waits for response
        User user = userServiceClient.getUserById(request.getUserId());
        
        if (user == null) {
            throw new UserNotFoundException();
        }
        
        // Validate user synchronously
        boolean isValid = userServiceClient.validateUser(user.getId(), 
                                                        new ValidationRequest());
        
        if (!isValid) {
            throw new InvalidUserException();
        }
        
        return processOrder(request, user);
    }
}

// Circuit breaker for resilience
@Component
public class UserServiceClientWithCircuitBreaker {
    
    @CircuitBreaker(name = "user-service", fallbackMethod = "fallbackGetUser")
    @TimeLimiter(name = "user-service")
    public CompletableFuture<User> getUserById(Long id) {
        return CompletableFuture.supplyAsync(() -> userServiceClient.getUserById(id));
    }
    
    public CompletableFuture<User> fallbackGetUser(Long id, Exception ex) {
        return CompletableFuture.completedFuture(new User(id, "Unknown User"));
    }
}
```

---

### Q33: Explain asynchronous communication in microservices. 📩

**Answer:** Asynchronous communication involves services communicating without waiting for an immediate response. This is typically achieved through message brokers, where a sender publishes a message, and receivers consume it independently. 📬

**Deep Explanation:** It's like sending an email: you send it and don't wait for a reply. This decouples services, improves resilience (messages can be queued if a service is down), and is ideal for event-driven architectures. 📨

**Real-time Use Case:** When an order is placed, the `Order Service` publishes an `OrderPlacedEvent`. The `Inventory Service`, `Payment Service`, and `Notification Service` all consume this event independently to perform their respective tasks. 📦

**Code Example:**
```java
// Event-driven architecture with Spring Cloud Stream
@Component
public class OrderEventPublisher {
    private final StreamBridge streamBridge;
    
    public OrderEventPublisher(StreamBridge streamBridge) {
        this.streamBridge = streamBridge;
    }
    
    public void publishOrderPlaced(Order order) {
        OrderPlacedEvent event = OrderPlacedEvent.builder()
            .orderId(order.getId())
            .customerId(order.getCustomerId())
            .items(order.getItems())
            .total(order.getTotal())
            .timestamp(Instant.now())
            .build();
            
        streamBridge.send("order-events", event);
    }
}

// Multiple services can consume the same event
@Component
public class InventoryEventHandler {
    
    @EventListener
    public void handleOrderPlaced(OrderPlacedEvent event) {
        // Reserve inventory items
        inventoryService.reserveItems(event.getOrderId(), event.getItems());
    }
}

@Component
public class PaymentEventHandler {
    
    @EventListener
    public void handleOrderPlaced(OrderPlacedEvent event) {
        // Process payment asynchronously
        paymentService.processPayment(event.getOrderId(), event.getTotal());
    }
}

@Component
public class NotificationEventHandler {
    
    @EventListener
    public void handleOrderPlaced(OrderPlacedEvent event) {
        // Send confirmation email
        notificationService.sendOrderConfirmation(event.getCustomerId(), 
                                                 event.getOrderId());
    }
}

// Kafka configuration
@Configuration
@EnableKafka
public class KafkaConfig {
    
    @Bean
    public ProducerFactory<String, Object> producerFactory() {
        Map<String, Object> props = new HashMap<>();
        props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
        props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, JsonSerializer.class);
        return new DefaultKafkaProducerFactory<>(props);
    }
    
    @Bean
    public KafkaTemplate<String, Object> kafkaTemplate() {
        return new KafkaTemplate<>(producerFactory());
    }
}
```

---

### Q34: What are the challenges of data management in microservices? 📊

**Answer:** Data management is challenging due to decentralized data ownership. This leads to issues with data consistency, distributed transactions, data duplication, and ensuring data integrity across services. 🤯

**Deep Explanation:** Since each service has its own database, maintaining strong consistency is difficult. Microservices often use **eventual consistency** and patterns like the **Saga pattern** to manage distributed transactions and ensure data integrity over time. 🚧

**Real-time Use Case:** In an e-commerce platform, placing an order involves multiple services (inventory, payment, order). A Saga orchestrates these steps and provides compensating actions to roll back changes if a step fails. 🛒

**Code Example:**
```java
// Saga pattern implementation
@Component
public class OrderSaga {
    
    @SagaOrchestrationStart
    public void processOrder(OrderCreatedEvent event) {
        // Step 1: Reserve inventory
        sagaManager.choreography()
            .step("reserve-inventory")
            .invokeParticipant("inventory-service")
            .withCompensation("release-inventory");
    }
    
    @SagaOrchestrationStep("inventory-reserved")
    public void processPayment(InventoryReservedEvent event) {
        // Step 2: Process payment
        sagaManager.choreography()
            .step("process-payment")
            .invokeParticipant("payment-service")
            .withCompensation("refund-payment");
    }
    
    @SagaOrchestrationStep("payment-processed")
    public void confirmOrder(PaymentProcessedEvent event) {
        // Step 3: Confirm order
        orderService.confirmOrder(event.getOrderId());
    }
    
    @SagaOrchestrationCompensation("release-inventory")
    public void releaseInventory(OrderFailedEvent event) {
        inventoryService.releaseReservation(event.getOrderId());
    }
    
    @SagaOrchestrationCompensation("refund-payment")
    public void refundPayment(OrderFailedEvent event) {
        paymentService.refundPayment(event.getPaymentId());
    }
}

// Event sourcing for data consistency
@Entity
public class OrderAggregate {
    @Id
    private String id;
    private List<DomainEvent> events = new ArrayList<>();
    
    public void placeOrder(OrderPlacedEvent event) {
        // Apply business logic
        this.events.add(event);
    }
    
    public void reserveInventory(InventoryReservedEvent event) {
        this.events.add(event);
    }
    
    public void processPayment(PaymentProcessedEvent event) {
        this.events.add(event);
    }
    
    // Replay events to rebuild state
    public static OrderAggregate fromEvents(List<DomainEvent> events) {
        OrderAggregate aggregate = new OrderAggregate();
        events.forEach(aggregate::apply);
        return aggregate;
    }
}
```

---

### Q35: What is an API Gateway in Microservices? 🚪

**Answer:** An API Gateway is a single entry point for all client requests to a microservices-based application. It acts as a reverse proxy, routing requests and handling cross-cutting concerns like authentication, rate limiting, and caching. 🛡️

**Deep Explanation:** An API Gateway simplifies client interactions by abstracting the internal microservice architecture. It can also aggregate responses from multiple services into a single response, reducing network calls for the client. 聚合

**Real-time Use Case:** A mobile app needing to display a product page makes a single call to the API Gateway. The gateway then internally calls the `Product`, `Review`, and `Pricing` services, aggregates their responses, and sends a single response back to the app. 📱

**Code Example:**
```java
// Spring Cloud Gateway configuration
@Configuration
public class GatewayConfig {
    
    @Bean
    public RouteLocator customRouteLocator(RouteLocatorBuilder builder) {
        return builder.routes()
            // Product service routes
            .route("product-service", r -> r.path("/api/products/**")
                .filters(f -> f
                    .addRequestHeader("X-Gateway", "Spring-Cloud-Gateway")
                    .circuitBreaker(c -> c.setName("product-service")
                        .setFallbackUri("forward:/fallback/products")))
                .uri("lb://product-service"))
            
            // User service routes
            .route("user-service", r -> r.path("/api/users/**")
                .filters(f -> f
                    .addRequestHeader("X-Gateway", "Spring-Cloud-Gateway")
                    .requestRateLimiter(c -> c
                        .setRateLimiter(redisRateLimiter())
                        .setKeyResolver(userKeyResolver())))
                .uri("lb://user-service"))
            
            // Order service routes with authentication
            .route("order-service", r -> r.path("/api/orders/**")
                .filters(f -> f
                    .addRequestHeader("X-Gateway", "Spring-Cloud-Gateway")
                    .filter(authenticationFilter()))
                .uri("lb://order-service"))
            
            .build();
    }
    
    @Bean
    public RedisRateLimiter redisRateLimiter() {
        return new RedisRateLimiter(10, 20); // 10 requests per second, burst of 20
    }
    
    @Bean
    public KeyResolver userKeyResolver() {
        return exchange -> exchange.getRequest().getHeaders()
            .getFirst("X-User-ID");
    }
}

// Custom authentication filter
@Component
public class AuthenticationFilter implements GatewayFilter {
    
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        ServerHttpRequest request = exchange.getRequest();
        
        if (!request.getHeaders().containsKey("Authorization")) {
            return onError(exchange, "Missing Authorization header", HttpStatus.UNAUTHORIZED);
        }
        
        String token = request.getHeaders().getFirst("Authorization");
        if (!isValidToken(token)) {
            return onError(exchange, "Invalid token", HttpStatus.UNAUTHORIZED);
        }
        
        return chain.filter(exchange);
    }
    
    private Mono<Void> onError(ServerWebExchange exchange, String err, HttpStatus httpStatus) {
        ServerHttpResponse response = exchange.getResponse();
        response.setStatusCode(httpStatus);
        return response.setComplete();
    }
    
    private boolean isValidToken(String token) {
        // Validate JWT token
        return jwtService.validateToken(token);
    }
}

// Response aggregation
@RestController
public class AggregationController {
    
    @GetMapping("/api/product-details/{id}")
    public Mono<ProductDetailsResponse> getProductDetails(@PathVariable String id) {
        Mono<Product> product = webClient.get()
            .uri("http://product-service/api/products/" + id)
            .retrieve()
            .bodyToMono(Product.class);
            
        Mono<List<Review>> reviews = webClient.get()
            .uri("http://review-service/api/reviews/product/" + id)
            .retrieve()
            .bodyToFlux(Review.class)
            .collectList();
            
        Mono<PricingInfo> pricing = webClient.get()
            .uri("http://pricing-service/api/pricing/" + id)
            .retrieve()
            .bodyToMono(PricingInfo.class);
            
        return Mono.zip(product, reviews, pricing)
            .map(tuple -> new ProductDetailsResponse(
                tuple.getT1(), // product
                tuple.getT2(), // reviews
                tuple.getT3()  // pricing
            ));
    }
}
```

---

*[Continue with remaining questions Q36-Q100 following the same detailed format...]*

---


Perfect for interview preparation, knowledge refresh, or learning microservices architecture! 🚀

---

*Happy Learning! 🎓*

