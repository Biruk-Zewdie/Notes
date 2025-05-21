# Inversion of control (IOC)
- is a design principle where object (bean) creation, lifecycle management, deletion and dependency injection is transferred/inverted from the developer to the container.
- bean creation and it's lifecycle is managed by spring IOC container
- traditionally we use new key work to create a been and also we inject it manually.

# Dependency Injection 
- The actual technique used by the container to provide one objects dependency to another.
- there are three main types of Dependency Injection.
1. constructor Injection
    - dependency is injected during instance creation 
    - immutability - fields can be final, ensuring they are set only once.
    - makes unit testing easier (with constructor mokes).
    - disadvantage - verbose and might occur circular dependency.
2. Setter injection.
    - can be injected only when needed - partial dependencies.
    - more readable for many dependencies.
    - allows for re-injection, change after construction.
    - disadvantages 
        - object can be partially initialized state (not safe until setters are called)
        - dependencies are mutable (can not use final fields)
        - less suitable for mandatory dependencies 
3. Field injection 
    - Very concise and easy to read.
    - no need for setters and constructors.
    - disadvantages (not recommended for production)
        - Breaks encapsulation - directly access fields instead of going through setters/ getters
        - difficult for unit test.


# what is the purpose of IOC 
1. Loose coupling - Class don't directly create their dependencies - easier to swap out implementation.
2. Better testability - we can inject mock dependencies for testing. 
3. Centralized Configuration - Object creation and wiring are handled in one place. - in IOC container.
4. Scalability and Maintainability - easier to manage in large systems with many interconnected systems 

# types of IOC Containers 
1. bean factory  
    - basic IOC container 
    - Lazy initialization - beans are created only when needed 
    - mostly used for lightWeight apps, limited memory.
    - requires configuration in bean.xml. so that it is very verbose and ugly.
2. Application context 
    - Advanced IOC container (supported internationalization, Event handling, AOP integration)
    - Eager initialization - beans are created at app startup by default 
    - Almost always used in Spring apps
    - ApplicationContext = BeanFactory + many additional features.
    - provides support for annotation so we don't have to use verbose xml based configuration but it would accept xml based configuration as well. 
    - E.g. ApplicationContext ac = new ClassPathXmlApplicationContext(applicationContext.xml);

- @SpringBootApplication = @ComponentScan + @Configuration + @EnableAutoConfiguration.

```
ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
Car car = context.getBean(Car.class);
```
or in spring boot app
```
@SpringBootApplication 
public class myApp {
    public static void main (String [] args){
        ApplicationContext context = SpringApplication.run(MyApp.class, args);
    }
}
```

# What is Spring bean?
- Spring bean is just an object that is created, lifecycle managed and deleted by IOC container.
- Spring bean = POJO + Spring configuration.
- Steps of bean creation in Application Context 
    - step 1 - scan or load configuration (scan for 4 stereotype annotation using @ComponentScan annotation)
    - step 2 - create object from class using reflection 
    - step 3 - inject Dependencies 
    - step 4 - Process bean lifeCycle hooks 
    - step 5 - store IOC container - store in bean registry 

- beans can be defined in three ways 
1. XML Configuration - via <bean> tag in ApplicationContext.xml file 
2. Configuration Java Class -  using @Configuration, @Bean And @Scope
3. Annotation Driven - using 4 stereotype annotations (@component-in model, @Repository - in DAO, @Service - in service and, @Controller - in controller)

## Note 
- Spring Framework will not treat any Class with these stereotype annotations differently from one another. They all do the same thing (Register a Class as a Bean). The difference in annotation names is purely for developer readability.

## @Autowire 
- is annotation that Auto-magically (abstracts away) determines the dependency we need and provides form IOC container.
- put the annotation above the constructor, setter or field and it depends on what type of injection we want. 

# Bean Scope 
- Bean scope refers to how many instance of a bean can be created and where they are used?
- there are 6 bean scopes
    - 2 universal scope (singleton and prototype) and 
    - 4 specifically web based (take HTTP requests) scopes. (request, session, web socket and application). 
## Singleton 
- only one instance of a bean created per spring container (ApplicationContext).
- this single instance is shared across the entire application when ever the bean is injected.
- springs default bean scope 

```
@Component
@Scope("singleton") // optional — singleton is the default
public class MyService {
    // business logic
}
```
## prototype 
- new instance of a bean every time it is injected or requested.

# Spring Boot
- Spring boot is a project that is used to create spring based application that we can run.
- spring boot is based on the mind set of convention over configuration. 
- abstracts away all of that verbose configurations (especially xml config)
## benefits of spring boot 
- Apache Tomcat server (similar to javalin but easer to configure).
- configurations in general are easy (no more XML config) in application.properties file.
- provide starter dependencies.
- spring boot actuator - endpoints for us to monitor metrics and project information for our running applications.
- Spring boot DevTools - to set up easier development environment.

## Spring MVC (AKA Spring web)
- MVC stands for Model-View-Controller.
- it is a design pattern which built on the Spring Framework that helps you build robust, loosely coupled web applications by separating responsibilities into three main components:
1. model (data/database and business logic layer) 
2. view (client/frontend) and 
3. Controller (backend logic, that connects the two).
    - receives http requests and sends back http responses.

    <img alt = "Spring MVC architecture" src = "https://www.javawebtutor.com/images/spring/spring-mvc-flow.png" width = "800">

### Annotations in Controller layer
1. @RestController - @Controller + @ResponseBody 
    - A shortcut annotation that combines @Controller and @ResponseBody.
    - ensures all methods return JSON or XML directly.
2. @Controller
    - One of Spring’s 4 stereotype annotations. 
    - Marks a class as a Spring MVC controller and registers it as a bean in the application context.
    - is not injected into anything — it is used by Spring, not used in Spring like services or DAOs are
3. @ResponseBody - Converts the return value of a method (Java object) into JSON (or XML) and sends it as the HTTP response body.
4. @RequestMapping - Maps incoming HTTP requests to specific controller methods based on path and HTTP method.
    - can be placed before a class declaration or above a method declaration. 
5. @CrossOrigin - Allows Cross-Origin Resource Sharing (CORS) by specifying allowed origins or ports to avoid the browser’s default same-origin policy blocking.
6. @PathVariable - Extracts values from URI path segments (e.g., /users/{id}).
7. @RequestParam - Extracts values from query parameters in the URL (e.g., /search?keyword=spring).
8. @RequestBody - Binds the HTTP request body (usually JSON) to a Java object.
9. @GetMapping, @PostMapping, @DeleteMapping, @PatchMapping - Shorthand annotations for @RequestMapping with specific HTTP methods.

## Dispatcher Servlet 
- also known as front controller.
- that sits at the front of the server and controls where the request go. 

 <img alt = "Spring MVC architecture" src = "https://media.geeksforgeeks.org/wp-content/uploads/20231106150237/Spring-MVC-Framework-Control-flow-Diagram.png" width = "500">

- Spring MVC Flow (Based on the Diagram)

1. When the Tomcat Server receives a HTTP request, it gets passed to the DispatcherServlet (our Front CoC ntroller)
2. The DispatcherServlet consults the HandlerMapping (which looks for @RequestMapping annotations) determine which controller to send the request to, based on the /endpoint of the HTTP request.
3. The Controller consults the service to process the request (service, dao, and DB do their thing)
4. The Controller builds the response, and hands it back to the DispatcherServlet
5. The DispatcherServlet then consults the ViewResolver if necessary to process a view (which we won’t use, since we will have our own frontend views)
6. The DispatcherServlet hands the response back to the Tomcat Server, which sends it to the front end (or wherever the request came from, maybe postman)
7. The HTTP lifecycle starts again whenever a new request is sent!

## HandlerMapping 
- an interface that is responsible for sending requests to the proper controller.
- tells a dispatcherServlet which controller takes which HTTP request.
- the most common implementation is **RequestMappingHandlerMapping**, which is what we will use.
- scans the application for @RequestMapping, @GetMapping, @PostMapping, etc

## ResponseEntity Object 
- is an object used to send responses to the client.
- let us to do things like status code and the body of the response.

# Spring Data JPA (AKA Spring Data)
- JPA stands for Jakarta Persistence API
- is a spring project that provides repository support for Java Persistence API using JDBC under the hood.
- spring Data abstracts away a framework called Hibernate, which abstracts away JDBC (Java Database Connectivity).
- Hibernate is ORM - Object Relational Mapping - object came from Java (OOP) and Relational came from relational database. so hibernate is 
- the DAO is about to be way simpler, we are going about to be able to do all our DDL from our model class.
- JPA is where we get the annotation that map our model class to DB tables. (@Entity). So our database tables get created by our java models. 
## EntityManager Interface
- is an interface that is used to perform CRUD operations in our database (table) and their data in a database.
- lets us query/manipulate our database from our java server.

## Spring Data’s advantages over JDBC
1. JDBC especially in large application can become very complex.
2. If we need to change our database dialect we would likely need to edit a significant amount of our JDBC statements for syntax reason. However spring data can do that for us just by changing the database dialect in our config. file (application.properties)
3. In JDBC we have to convert resultSets manually to our POJOs. Spring data will do that for us.
4. JDBC requires the developer to have a specific knowledge of the database while spring data does not.
5. Spring Data requires much less line of connection code and DAO code.

### Spring Data Interface Hierarchy 
- from top level generic interface to the most common one
1. Repository <T, ID>
    - Top-level marker interface.
    - No methods defined — just marks the interface for Spring Data to detect.
2. CrudRepository<T, ID> extends Repository<T, ID>
    - Provides most basic CRUD operations such as 
         - Save (S entity) - create 
         - FindById (ID id) - read
         - FindAll () - read 
         - deleteById (ID id) - delete 
         - existsById (ID id)
         - count ()

3. PageAndSortingRepository<T, ID> extends CrudRepository <T, ID>
    - Adds pagination and sorting 
        - FindAll (Sort sort)
        - FindAll (Pageable pageable)
4. JpaRepository <T, ID> extends PageAndSortingRepository <T, ID> 
    - Adds JPA specific features 
        - flush ()
        - saveAndFlush ()
        - deleteInBatch ()
        - findAll (Specification spec)
        - getOne (ID id) - lazy loading proxy 
5. our custom Interface extending one of the above.

```
@Repository
public interface JobDAO extends JpaRepository<Job, UUID> {

    Optional<List<Job>> findByEmployer_EmployerId(UUID employerId);

    List <Job> findByJobTitleContainingIgnoreCase(String jobTitle);
    List <Job> findBySalaryBetween(double minSalary, double maxSalary);
    List <Job> findByJobType (Job.JobType jobType);
}
```
- there are custom query methods that are generated by spring based on naming rules. 

### Fundamental JPA Annotations in our Java Classes
1. @Component - one of the 4 stereotype annotation that create a class a bean in our Model class.
2. @Repository - one of the 4 stereotype annotation that creates a class a bean in our DAO class
3. @Entity -  marks a class as JPA entity and used to map a class to a database table using JPA.
4. @Table - used to set a name of a table. By default a name of a table is a class name.
5. @Id - declares the field as a primary key in the table.
6. @GenerateValue - gives us an option for how hibernate autogenerate a primary key.  
7. @Column - hibernate converts all fields in our class as a table columns by default but using this annotation lets us set things such as column name, or constraints like not null, unique and so on.

## JPA Relationship annotation 
-  this annotations define the relationship between our model classes in java. Which as we know mapping between tables using Foreign keys. 
1. @OneToOne - Defines a one-to-one relationship between two entities.
    - Example: A User has one Profile, and a Profile belongs to one User.
2. @ManyToMany
    - A @ManyToMany relationship in JPA is used when multiple records of one entity are associated with multiple records of another entity.
    - example Student and Course
        - A student can enroll in many courses and a course can have many students.
    - We must create a third table, often called a junction table or join table, because SQL databases (like PostgreSQL, MySQL, SQLite) can’t directly represent many-to-many relationships between two tables without it.
    ```
    @Entity
    public class Student {
    
    @Id
    @GeneratedValue
    private Long id;

    private String name;

    @ManyToMany
    @JoinTable(
        name = "student_course", // junction table name
        joinColumns = @JoinColumn(name = "student_id"), // foreign key in the join table referring to Student
        inverseJoinColumns = @JoinColumn(name = "course_id") // foreign key in the join table referring to Course
    )
    private List<Course> courses;
    }
    ```
3. @OneToMany 
    - Placed on the field in the class that represents the “one” side of the relationship.
	- That field typically holds a collection (e.g., List, Set).
    - Example: A Customer can have many Orders. So, in the Customer class:
    ```
    @OneToMany(mappedBy = "customer")
    private List<Order> orders;
    ```
4. @ManyToOne 
    - 	Placed on the field in the class that represents the “many” side.
    - 	Example: Many Orders belong to one Customer.
    ```
    @ManyToOne
    private Customer customer;
    ```
    #### **CascadeType** 
    - specifies the cascading on the database when removing/updating items. 
    - This is how we maintain referential integrity in hibernate.
    #### **FetchType**
    - determines if the related objects are called from the database immediately (EAGER) or if called, waits until they are needed (LAZY). 

```
@ManyToOne(fetch = FetchType.EAGER, cascade = CascadeType.ALL)
@JoinColumn(name = "customerId")
private Customer customer;
```
### Other annotations under relationships 
1. @JoinColumn 
- specifies a column to establish a relationship on.
- Just like we do normally, this is usually a Foreign Key pointing to the Primary Key of the table being referred to. 




     