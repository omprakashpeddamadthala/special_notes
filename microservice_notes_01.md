

### 1. What is a Microservice? Explain its advantages by comparing it with Monolithic architecture.

In simple terms, a **microservice** is an approach to building a single application as a suite of small, independent services. Each service runs in its own process and communicates with others over a network.

Think of your **Threadvine** project. Instead of building one giant eCommerce application, you built it as separate services like `User Service`, `Order Service`, and `Catalog Service`.

| Feature | Monolithic Architecture (One Big Application) | Microservices Architecture (e.g., Threadvine) |
| :--- | :--- | :--- |
| **Development** | The entire team works on a single large codebase. A change in one part can affect the whole system. | Small, focused teams can work independently on different services (`User Service` team, `Order Service` team). This speeds up development. |
| **Deployment** | You have to redeploy the entire application even for a small change. This is risky and slow. | You only need to redeploy the service you changed. For example, if you update the discount logic, you only redeploy the `Order Service`. This is faster and safer. |
| **Technology** | The entire application is built with one technology stack (e.g., all Java). | Each service can use the best technology for its job. Your `User Service` could be in Java, while a future `Recommendation Service` could be in Python for machine learning. |
| **Scalability** | You have to scale the entire application, even if only one part is under heavy load. | You can scale individual services. If many users are browsing products, you can scale only the `Catalog Service` without touching others. |
| **Fault Isolation** | If one feature fails, the entire application can go down. | If the `Notification Service` fails, customers can still place orders. The rest of the system continues to function. |

### 2. What is the SAGA design pattern?

The SAGA pattern is a way to manage data consistency across multiple microservices when you can't use a traditional database transaction. It's a sequence of local transactions. If one transaction fails, the SAGA pattern executes a series of compensating transactions to undo the changes made by the preceding transactions.

**Real-time Situation in Threadvine:**

Consider the **Order Placement** flow:
1.  **`Order Service`**: Creates an order and sets its status to "PENDING".
2.  **`Customer Service`**: Deducts the order amount from the customer's wallet or credit.
3.  **`Catalog Service`**: Decrements the stock for the ordered items.

What if the stock decrementation (Step 3) fails? The customer has been charged, but the order can't be fulfilled.

With SAGA, you would trigger compensating transactions:
*   **`Catalog Service`** fails -> It notifies the SAGA orchestrator.
*   **`Customer Service`**: A compensating transaction is called to refund the customer's wallet.
*   **`Order Service`**: A compensating transaction is called to change the order status to "FAILED".

This ensures the system returns to a consistent state.

### 3. Explain a Microservice created in your project.

In my project, **Threadvine**, I extensively worked on the **Order Service**.

This service is responsible for all business logic related to customer orders. Its primary functions are:
*   **Order Creation**: It provides an API endpoint for customers to place new orders.
*   **Order Processing**: It manages the lifecycle of an order, from "Pending" to "Shipped" and "Completed".
*   **Business Rule Implementation**: This was a key feature I developed. For example, it applies a 10% discount if the customer belongs to a specific "buyer group" (Order Header Rule) or sets a fixed line price if the order amount is within a certain range (Order Item Rule).
*   **Communication with Other Services**:
    *   It calls the **`User Service`** to get customer details.
    *   It calls the **`Catalog Service`** to verify product prices and stock.
    *   It publishes events to **Kafka** (e.g., `order_placed`) so the **`Notification Service`** can send an email to the customer via SendGrid.

It has its own dedicated database to store all order-related data, ensuring it is decoupled from other services.

### 4. What are the design patterns in Microservices?

Besides the **SAGA** pattern, some other common design patterns we used or considered in **Threadvine** are:

*   **API Gateway**: A single entry point for all client requests. We used this in our project.
*   **Service Discovery & Registry**: Helps services find and talk to each other. We used Eureka Server for this.
*   **Circuit Breaker**: Prevents a network or service failure from cascading to other services.
*   **Database per Microservice**: Each microservice has its own private database. Your `User Service`, `Order Service`, and `Catalog Service` each had their own database.
*   **Event-Driven Architecture**: Services communicate asynchronously through events, which we did using Kafka.
*   **Strangler Fig Pattern**: Used when migrating a monolith to microservices, where you gradually replace old functionality with new microservices.

### 5. What is an API Gateway?

An API Gateway is a server that acts as a single entry point for all client requests to the application's microservices. In your project, this was the **API Gateway** service.

Instead of a client (like a web browser or mobile app) having to know the addresses of all 10 microservices, it makes a single call to the API Gateway. The Gateway then routes the request to the appropriate microservice (`User Service`, `Order Service`, etc.).

**Advantages in Threadvine:**
*   **Simplifies Client Code**: The frontend only needs to know one URL—the Gateway's.
*   **Centralized Concerns**: It handles cross-cutting concerns like authentication (checking if the user is logged in), rate limiting, and logging in one place.
*   **Routing**: It intelligently routes requests. A request to `/api/orders` goes to the `Order Service`, while `/api/users` goes to the `User Service`.

### 6. What is a circuit breaker and explain its each state?

A circuit breaker is a design pattern that prevents an application from repeatedly trying to call a service that is down. This stops failures from cascading and overwhelming the system.

Imagine the `Order Service` needs to call the `Catalog Service` to get product details. If the `Catalog Service` is down, the `Order Service` might keep trying, using up resources and becoming slow or unresponsive itself.

The circuit breaker wraps these calls and has three states:

1.  **CLOSED**: This is the normal state. Requests are allowed to pass through to the other service (e.g., `Order Service` calls `Catalog Service`). The circuit breaker monitors for failures. If the number of failures exceeds a threshold, it trips and moves to the **OPEN** state.

2.  **OPEN**: The circuit is "tripped." For a configured amount of time, all requests to the failing service are immediately rejected without even trying to call it. This gives the failing service time to recover. After the timeout, it moves to the **HALF-OPEN** state.

3.  **HALF-OPEN**: In this state, the circuit breaker allows a single "trial" request to go through to the failing service.
    *   If this trial request **succeeds**, the circuit breaker assumes the service has recovered and moves back to the **CLOSED** state.
    *   If the trial request **fails**, it goes back to the **OPEN** state and the timer starts again.

### 7. What is Event-Driven Architecture?

Event-Driven Architecture (EDA) is a model where services communicate by producing and consuming events. An "event" is a significant change in state, like "Order Placed" or "User Registered."

This promotes loose coupling. The service that produces the event (the publisher) doesn't know which services will consume it.

**Real-time Situation in Threadvine:**
We used **Kafka** to implement this.
1.  **Event Production**: When a user places an order, the `Order Service` publishes an `OrderPlacedEvent` to a Kafka topic named `order_events`.
2.  **Event Consumption**:
    *   The **`Notification Service`** listens to this topic. When it sees the event, it sends a confirmation email.
    *   The **`User Activity Service`** also listens. It logs that the user has placed an order.

The `Order Service` doesn't need to directly call these other services. It just announces what happened, and other services react accordingly.

### 8. Steps to implement RabbitMQ/Kafka?

In our project, we used **Kafka**. Here are the basic steps to implement it in a Spring Boot application:

1.  **Add Dependencies**: Add the Spring for Apache Kafka dependency to your `pom.xml` or `build.gradle`.

    ```xml
    <!-- pom.xml -->
    <dependency>
        <groupId>org.springframework.kafka</groupId>
        <artifactId>spring-kafka</artifactId>
    </dependency>
    ```

2.  **Configure Kafka**: In your `application.properties` or `application.yml`, specify the Kafka broker address.

    ```properties
    # application.properties
    spring.kafka.bootstrap-servers=localhost:9092
    spring.kafka.consumer.group-id=my-group
    ```

3.  **Create a Producer (Publisher)**: To send messages, you use the `KafkaTemplate`.

    **Example in `Order Service`:**
    ```java
    @Service
    public class OrderProducer {
        private static final String TOPIC = "order_events";
        @Autowired
        private KafkaTemplate<String, String> kafkaTemplate;

        public void sendOrderPlacedEvent(String orderDetailsJson) {
            System.out.println("Publishing order event: " + orderDetailsJson);
            this.kafkaTemplate.send(TOPIC, orderDetailsJson);
        }
    }
    ```

4.  **Create a Consumer (Listener)**: To receive messages, you use the `@KafkaListener` annotation.

    **Example in `Notification Service`:**
    ```java
    @Service
    public class NotificationConsumer {
        @KafkaListener(topics = "order_events", groupId = "notification_group")
        public void consume(String message) {
            System.out.println("Received order event for notification: " + message);
            // Logic to send email using SendGrid
        }
    }
    ```

### 9. What are the points that need to be considered while designing or migrating towards a Microservices architecture?

1.  **Bounded Context**: How do you break down the monolith? Services should be designed around business capabilities (e.g., `Orders`, `Users`, `Catalog`). Each service should own its data and logic.
2.  **Data Consistency**: Since each service has its own database, you can't have traditional ACID transactions across services. You need to plan for eventual consistency using patterns like SAGA.
3.  **Communication**: Will communication be synchronous (REST API) or asynchronous (Kafka, RabbitMQ)? We used a mix in **Threadvine**: REST for immediate requests and Kafka for background tasks like notifications.
4.  **Service Discovery**: How will services find each other? You need a service registry like Eureka.
5.  **API Gateway**: You need a single entry point to avoid exposing all your services to the client.
6.  **Operational Complexity**: You are now managing 10 applications instead of one. This requires robust CI/CD, monitoring, and logging. We used Docker, Jenkins, and AWS to manage this.
7.  **Fault Tolerance**: What happens when a service is down? You need to implement patterns like Circuit Breakers and retries.

### 10. What is Kubernetes?

Kubernetes (often called K8s) is an open-source container orchestration platform. Its job is to automate the deployment, scaling, and management of containerized applications.

Since your **Threadvine** application was deployed on AWS using Docker, Kubernetes would be the next logical step to manage those Docker containers at scale. It handles things like:
*   **Scheduling**: Automatically finding a server to run your container.
*   **Self-healing**: Restarting containers that fail.
*   **Scaling**: Automatically increasing or decreasing the number of containers running based on traffic.

### 11. What are pods in Kubernetes?

A **Pod** is the smallest and simplest deployable unit in Kubernetes. A Pod represents a single instance of a running process in your cluster.

*   A Pod contains one or more containers (like Docker containers).
*   Typically, you have one main container per Pod. For example, you would have a Pod for your `User Service` that runs the `User Service` Docker container.
*   All containers within a single Pod share the same network space and can communicate with each other directly.

### 12. What is Docker? Explain how to create a docker image.

**Docker** is a platform that allows you to package an application and all its dependencies (libraries, configuration files, etc.) into a single, standardized unit called a **container**. This container can then run on any machine that has Docker installed, ensuring it works the same everywhere.

We used Docker to containerize each of our 10 microservices in the **Threadvine** project.

**How to create a Docker image:**

You create an image by writing a `Dockerfile`. This is a simple text file with instructions.

**Example `Dockerfile` for the `User Service`:**
```dockerfile
# 1. Start with a base image that has Java installed
FROM openjdk:11-jre-slim

# 2. Set the working directory inside the container
WORKDIR /app

# 3. Copy the compiled Java application (the .jar file) into the container
COPY target/user-service-0.0.1-SNAPSHOT.jar user-service.jar

# 4. Expose the port the Spring Boot application runs on
EXPOSE 8081

# 5. The command to run when the container starts
ENTRYPOINT ["java", "-jar", "user-service.jar"]
```

To build the image, you run this command in your terminal:
```bash
docker build -t threadvine/user-service .
```
This command reads the `Dockerfile` and creates a Docker image named `threadvine/user-service`.

### 13. What is serverless deployment?

Serverless deployment (or Serverless Computing) is a cloud execution model where the cloud provider (like AWS) dynamically manages the allocation and provisioning of servers. You write your code as functions and the cloud provider runs them for you.

You don't have to worry about the underlying infrastructure (servers, operating systems). You only pay for the actual compute time your function uses.

**Real-time Situation in Threadvine:**
While the main services were deployed on servers (via Docker/AWS), a feature like **sending an application error alert email** could be perfect for serverless. Instead of having the `Notification Service` handle it, you could use an **AWS Lambda** function. When an error occurs, it triggers the Lambda function, which then uses SendGrid to send the email. This is very cost-effective for infrequent tasks.

### 14. What protocols are used to communicate between two spring boot services?

In a microservices architecture like **Threadvine**, services primarily communicate using two types of protocols:

1.  **Synchronous Protocol (Request-Response)**:
    *   **HTTP/REST**: This is the most common method. One service makes a direct HTTP request (e.g., GET, POST) to another service's API endpoint and waits for a response.
    *   **Example**: When a user places an order, the `Order Service` might make a synchronous REST call to the `User Service` to get the customer's shipping address. It needs this information immediately to proceed.

2.  **Asynchronous Protocol (Event-Based)**:
    *   **AMQP/STOMP (with RabbitMQ) or Kafka Protocol**: This is used for messaging. A service sends a message to a message broker (like Kafka) and doesn't wait for a response. Other services can subscribe to these messages and process them when they are ready.
    *   **Example**: We used this with **Kafka**. The `Order Service` publishes an `OrderPlaced` event. The `Notification Service` consumes this event later to send an email. The `Order Service` doesn't have to wait for the email to be sent.

### 15. What is idempotent in Microservices?

An operation is **idempotent** if making the same request multiple times produces the same result as making it just once. This is very important in distributed systems where network failures can cause a client to retry a request.

**Real-time Situation in Threadvine:**
Imagine the frontend sends a request to the `Order Service` to submit an order. Due to a network glitch, the frontend doesn't get a confirmation and sends the *exact same request* again.

*   **If the operation is NOT idempotent**: The system might create two identical orders for the customer. This is a major problem.
*   **If the operation IS idempotent**: The `Order Service` should be designed to recognize that this is a duplicate request. It could check for an order with the same unique request ID. It would ensure the order is created only once and would simply return the details of the already-created order on the second attempt.

### 16. How to achieve asynchronous communication in a Microservice?

Asynchronous communication is achieved by using a **message broker**. The general flow is:
1.  A service (the **Producer**) sends a message to the message broker.
2.  The message is stored in a **queue** or a **topic** within the broker.
3.  Another service (the **Consumer**) subscribes to that queue/topic and processes the message whenever it's ready.

In our **Threadvine** project, we achieved this using **Apache Kafka**:
*   **Producer**: The `User Service`, when a new user registers, publishes a `UserRegisteredEvent` to a Kafka topic.
*   **Message Broker**: Kafka receives and stores this event.
*   **Consumer**: The `Notification Service` is listening to that topic. It consumes the event and calls the SendGrid API to send a welcome email.

This decouples the services. The `User Service` doesn't have to wait for the email to be sent and is not affected if the `Notification Service` is temporarily down.

### 17. How to implement spring cloud in java spring boot?

Spring Cloud is a collection of tools that helps developers quickly build common patterns in distributed systems (like microservices). To implement it, you typically add a "Bill of Materials" (BOM) to your `pom.xml` to manage dependency versions, and then add the specific starters you need.

**Steps:**
1.  **Add Spring Cloud BOM**: In your `pom.xml`, add the `dependencyManagement` section.

    ```xml
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>2021.0.3</version> <!-- Use a specific Spring Cloud version -->
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
    ```

2.  **Add Specific Starters**: Now, add the starters for the features you want. For example, in **Threadvine**:
    *   For the **Eureka Server** (Service Registry):
        ```xml
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>
        ```
    *   For a client service (like `Order Service`) to register with Eureka:
        ```xml
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        ```
    *   For the **API Gateway**:
        ```xml
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-gateway</artifactId>
        </dependency>
        ```

3.  **Enable with Annotations**: In your main Spring Boot application class, you add annotations to enable these features.
    *   On the Eureka Server: `@EnableEurekaServer`
    *   On a client service: `@EnableEurekaClient`

### 18. Which library in Java spring boot is used to implement Circuit breaker?

The most common library used to implement the Circuit Breaker pattern in Spring Boot is **Spring Cloud Circuit Breaker with Resilience4j**.

Previously, Netflix Hystrix was popular, but it is now in maintenance mode. Resilience4j is the modern, recommended choice.

**Implementation:**
1.  **Add Dependency**:
    ```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-circuitbreaker-resilience4j</artifactId>
    </dependency>
    ```
2.  **Apply Annotation**: You wrap the method that makes the external call with the `@CircuitBreaker` annotation and provide a "fallback" method.

    **Example in `Order Service` calling `Catalog Service`:**
    ```java
    @Service
    public class CatalogServiceClient {

        @Autowired
        private RestTemplate restTemplate;

        @CircuitBreaker(name = "catalogService", fallbackMethod = "getFallbackProductDetails")
        public Product getProductDetails(String productId) {
            // This might fail if Catalog Service is down
            return restTemplate.getForObject("http://catalog-service/products/" + productId, Product.class);
        }

        // This method is called if the circuit is OPEN
        public Product getFallbackProductDetails(String productId, Throwable t) {
            System.out.println("Fallback for catalog service. Error: " + t.getMessage());
            // Return a default or cached product
            return new Product("default-product", "Default Product Name", 0.0);
        }
    }
    ```

### 19. What is service discovery and service registry?

In a microservices architecture, services often run on multiple machines with dynamic IP addresses. Service Discovery is the process of how services find each other on the network.

*   **Service Registry**: This is a central server that acts like a phonebook for your microservices. When a service (like the `User Service`) starts up, it registers itself with the Service Registry, providing its name ("user-service") and its location (IP address and port). In your project, the **Eureka Server** was your Service Registry.

*   **Service Discovery**: When another service (like the `Order Service`) wants to talk to the `User Service`, it doesn't need to know its exact IP address. It simply asks the Service Registry: "Where can I find the 'user-service'?" The registry provides the location, and the `Order Service` can then make the call. This makes the system resilient to services moving around or scaling up and down.

### 20. Explain about API gateway and how it is implemented in your project?

As mentioned before, the **API Gateway** is the single entry point for all external requests. In our **Threadvine** project, it was a dedicated Spring Boot application.

**How it was implemented:**

1.  **Technology**: We used **Spring Cloud Gateway**.
2.  **Dependencies**: We added the `spring-cloud-starter-gateway` and `spring-cloud-starter-netflix-eureka-client` dependencies to its `pom.xml`.
3.  **Configuration**: The core of the API Gateway is its route configuration. We defined rules in our `application.yml` that mapped URL paths to specific microservices.

    **Example `application.yml` for the API Gateway:**
    ```yaml
    spring:
      cloud:
        gateway:
          discovery:
            locator:
              enabled: true # Enable discovery from Eureka
              lower-case-service-id: true
          routes:
            - id: user_service_route
              uri: lb://user-service # "lb" means load-balance via Eureka
              predicates:
                - Path=/api/users/**

            - id: order_service_route
              uri: lb://order-service
              predicates:
                - Path=/api/orders/**

            - id: catalog_service_route
              uri: lb://catalog-service
              predicates:
                - Path=/api/catalogs/**, /api/styles/**
    ```
    This configuration tells the gateway:
    *   Any request starting with `/api/users/` should be forwarded to an instance of the `user-service` found via Eureka.
    *   Any request starting with `/api/orders/` should go to the `order-service`.

The gateway also handled authentication by integrating with our **Authz Service**. It would check for a valid token on incoming requests before routing them to the internal services.

### 21. Can we connect Multiple databases in a Microservices? If yes, how?

Yes, absolutely. In fact, the standard practice for microservices is **Database per Service**. Each microservice should have its own private database that only it can access directly.

In your **Threadvine** project:
*   The **`User Service`** would have its own database with `users` and `roles` tables.
*   The **`Order Service`** would have its own database with `orders` and `order_items` tables.
*   The **`Catalog Service`** would have its own database with `products` and `catalogs` tables.

This is a core principle. You do **not** have multiple services sharing a single database, as that creates tight coupling and defeats the purpose of microservices. If the `Order Service` needs user information, it should not query the user database directly. Instead, it should ask the `User Service` for that data via an API call.
