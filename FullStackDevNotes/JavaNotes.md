# Java Interview Practice Note
## static keyword in Java
- can be used with members of a class variables, methods, blocks and nested class.
- to be associated with the class rather than with instance of a class.
- saves memory by sharing a single variable across all instance of a class.
- static methods can not access non static/ instance variables or methods directly. It can only use static variables and other non-static methods.
```
class Example {
    int x = 5;
    static void show() {
        Example obj = new Example();
        System.out.println(obj.x); // Correct: Access through object
    }
}
```
- static methods are called without creating an object, making them efficient for utility methods. 
- The **static blocks** is used to initializing a static variables before the main method runs. It executes only once when the class is loaded into memory. 
-  Useful for one-time initialization before the main method runs.
- A static nested class is a class defined inside another class using static. It does not need an instance of the outer class to be used.
## Non access modifiers 
- Non access modifiers are modifiers that define the behavior, properties, lifecycle of a variable, methods and classes.
- this includes: 
    - final 
    - static 
    - abstract 
    - synchronized
    - volatile 
    - transient  
    - scriptfp
    - native
## **1. Final Keyword**
- Can be used with class, method and variable/fields 
- It makes a fields and methods immutable or constant.
- if a field is final is must be initialized directly or using constructors otherwise it will throw an error
- methods with final keyword can not be overridden 
- class with final keyword can not be inherited/subclassed.
- there are only two ways in java to prevent classes from inherited
    1. using final keyword in class declaration 
    2. using sealed keyword in class declaration (we can permit subclasses that are allowed to inherit).

### Example
`public sealed class Shape permits Circle, Square {}`

- spring beans always use final keyword in our service and controllers class because 
    1. to make a bean immutable or safe 
    2. must be initialized or must not be null.
    3. better testability 
        - final + constructor injection = easy mocking during testing 
    4. cleaner code


## **2. Static** 
- the method or field is belongs to the class, not each instance.
- Every instance will share the same field value. 
- if the static field is changed, every instance of will get the changed value.
- instance variables/fields can not be accessible with in static methods.
- However, a static variable can be accessible within instance methods.

## **3. Synchronized**
- Synchronization is a mechanism to control access to shared resources in a multi-threaded environment to prevent race condition. 
- **race condition:** causing data inconsistency (happens when each threads tries to access and change data).
- solution - only one thread can access resource at a time.
- this can be achieved by using **Synchronized** keyword with methods and blocks.
- insure thread safety.
- both visibility as well as synchronization.

## **4. Volatile**
- is used to make a new value immediately visible to all other threads when one thread modifies a value.
- visibility of latest value between threads.
- Volatile is only used with fields because fields holds the value of states.
- The downside is that it does not provide atomicity or synchronization for compound operations like counter++.

## **Errors**
- errors are serious issues that can not be handled by the program.
- occurs due to system failure (hardware issue or serious programming mistakes) such ass memory OverFlow, JVM crash 
- extends throwable class
    ### Example
        - StackOverFlowError
        - OutOfMemoryError 
        - virtualMachineError

## **Exceptions**
- In java exceptions are problems that affects the normal program flow.
- can be checked exception (compile-time exception) and unchecked exception (runtime exception).
- both extends Exception class 
## **1. Checked Exception**
- happens due to recoverable issues.
- checked exceptions should be handled otherwise our code can't be compiled.
     - **Examples** 
        - IOException 
        - SQLException
        - fileNotFoundException
        - classNotFoundException
## **2. Unchecked Exception**
- not checked at compile time.
- unchecked Exceptions are exceptions that would occur due to logic errors.
- extends runtimeExceptions class.
- handling them is not mandatory but can improve user experience and prevents crashing.
- **Examples**
    - NullPointerExceptions
    - ArithmeticExceptions 
    - ArrayOutOfBoundExceptions
    - classCastException 

## Custom Exception
- Allows you to create meaningful message and specific to the business logic.

## when to use checked Vs unchecked Exception?
- checked exception is used when the exception is recoverable. (example - invalid input).
- unchecked exception is used when there is programming bug (ex - null Pointer).

## Comparing Objects in java
- Reference Comparison using **double equal sign** (obj1 == obj2).
- content Comparison using .equals() method (obj1.equals(obj2)).
- The default implementation (from Object class) behaves like ==, but it can be overridden in custom classes.

## Polymorphism
- the behavior of an object to exist in many forms.
- allows objects of different classes to be treated as objects of common superclass.
    ## example
        ```
        //Animal class
        public class Animal {
            public void makeSound () {
                 System.out.println("Animal make sound")
            }
        }

        //Dog class
        Public class Dog extends Animal {

            public void makeSound () {
                System.out.println("Dog barks")

            }
        }

        Animal dog = new Dog()
        ```
- polymorphism can be achieved in to ways 
    1. method overloading
    2. method overriding

1. method overloading
    - occurs in the same class
    - when multiple methods or constructors (spacial type of method) in a class can have the same method name but have different method signature/parameter list(a unique identifier of the method, which includes the method name and the sequence and types of its parameters).
    - the parameters list can be different in
        1. number of parameters 
        2. data type of parameters 
        3. order of parameters.
- since the selection of a method can be done during compilation based on the method signature it is called compile time polymorphism.
- overloaded methods can have different return types.

2. method overriding 
    - happens between child and parent class.
    - the methods in the child class have the same method signature as the parent class but different implementation detail from the parent class.
    - java decides which method to call based on the actual object's type at runtime. Therefore it is called runtime polymorphism.
    - overridden class in child class can not have more restrictive access modifier than parent class method.
    - the return type of the subclass must be the same or covariant(overriding method can refine the return type to more specific subtype).

## public static void main (String [] args){}
- the main method is java is an entry point java application. 
- (String [] args): used to pass command line arguments.
- valid change of order between the keywords is 
    ```
    public static void main (String [] args)
    static public void main (String [] args) ..... java doesn't care about the order of modifiers
    public static void main (String args [])
    ```

## what is the difference between final, finally and finalize?
- final - a non access modifier that is used to make fields/variables constant. Restricts modification in variables, methods (restrict overriding) and classes (restrict inheritance). 
- finally - is a block in exception handling, which cleans up our code or everything in our finally block will be always executed after a try-catch.
- finalize - a protected method in Object class that garbage collectors call on an object when it determines that the are no more references to the object.
    - it allows the object to perform clean up actions before being garbage collected. 

## what is singleton in java.
- So singleton is a design pattern in java that a class has only one instance through out the lifecycle of an application and has a global access.
    #### how to achieve singleton DP in java?
    - by making our constructor a private, so that we restrict the initiation of a class from other classes.
    - private static variable that is the only instance of the class.
    - public static method that returns the instance of the class - this is the global access point for the outer world to get the instance of the singleton class.

    ### Different implementation of singleton DP?
    1. Eager initialization - simple and thread-safe, but instance is created even if it is not used.

        ```private static final Singleton INSTANCE = new Singleton(); // Instance created at class loading.```

    2. Lazy instantiation (thread unsafe) - Saves memory by creating an instance only when needed. but it is not thread safe (multiple threads create multiple instances.)

    ```
    class Singleton {
        private static Singleton instance;
        private Singleton() {} // Private constructor
        public static Singleton getInstance() {   //to make it trade safe we can add a modifier synchronized 
            if (instance == null) { // Instance created only when needed
                instance = new Singleton();
            }
        return instance;
        }
    }
    ```
## Garbage collection
- garbage collection in java is the process in the JVM that automatically removes unused objects (objects that has no reference) from memory to free up space and relevant memory leaks. 
- The JVM decides when to run the Garbage collector based on memory usage/when the heap memory is full.
- we (the developer) can't force garbage collection, but we can suggest it using <span style="font-size:18px; color:#7190f6">**System.gc()**</span>.  

## unit testing
- is a technique in testing that individual units (methods or classes) of a software are tested in isolation to ensure they work correctly.
- the purpose of unit testing includes: 
    - detect bugs early.
    - improve code maintainability
    - helps with refactoring without breaking functionality.
    - supports test-driven development. 
- Junit-most commonly used framework for unit testing
- steps to perform unit testing using Junit
    1. Add Junit dependency (maven/gradle).
    2. Write test cases in a separate test class.
        - @Test - marks the method as a test case.
    3. Use assertions (assertTrue (), assertEquals (), etc) to validate expected results.
        - assertEquals(expected, actual) - checks if the result is correct.
    4. Run tests using IDE

## Array Vs ArrayLists
### Arrays
- An array is a contiguous block of memory that stores a collection of elements of the same data type.
- Elements are stored in the order of insertion, and can be accessed using zero-based indexing.
- The size of an array is fixed and must be specified when the array is created.
- wYou cannot remove an element directly from an array. Instead, you can set an element to null (for objects) or a default value (like 0 for integers), but the array size remains unchanged.
- You can reassign an element using its index, but since arrays in Java are fixed-size, there is no built-in method to actually remove an element and shift the rest.
- using arrays can be advantageous when 
    - the number of elements are known and fixed.
    - we want fast indexed access.
    - when memory efficiency is important. (arrays are less overhead than arrayLists)
-supports multi-dimensional arrays.

### ArrayLists
- it is one of the three classes that implement a list interface
- iAn ArrayList is a resizable array — it maintains the order of insertion and can dynamically grow or shrink.
- ArrayList stores objects only — for primitive types, you must use wrapper classes (Integer, Double, Boolean, etc.)
- It provides fast random access (by index) but is slower for insertions/removals in the middle of the list (because elements have to shift)
- Conversion 
    ## **Array to list** 

    ```String[] arr = list.toArray(new String[0]);```

    ## **List to array** 
    1. use Arrays.asList() method

        ```
        String [] arr = {Java, C++, Spring, React}
        List <String> list = Arrays.asList(arr);
        ```
    #### Note 
    - the resulting list is fixed sized - we can't add or remove an element but we can update a value.

    2. For modifiable list

    ```
    List <String> ModifiableList = new ArrayList<>(Arrays.asList(arrays)) 
    ```

# Thread
- Thread is the smallest unit of execution within a process.
- In java, thread allows a program to run multiple tasks concurrently.
- Multiple things happen at the same time (run parallelly), with the help of threads. Example video games.
- In order to achieve simultaneous execution of a program, We have to create multiple threads and this can be achieved by 
    1. implementing the runnable interface
        - runnable interface is a functional interface which only contains run method.
        - The main advantage of implementing the Runnable interface instead of extending the Thread class is that it allows a class to extend another class as well.
        - Since Java does not support multiple inheritance with classes, extending Thread would prevent you from extending any other class.
        - By implementing Runnable, you maintain the flexibility to inherit from another class while still enabling multithreaded behavior.
    2. extending the thread class. (this makes extending class a thread class)
- we will start a new thread with a .start() method. (start method call a run() internally).
- sleep (ms) - put a thread into sleep for a given milliseconds. 

    ### Multi-threading 
    - running multiple threads concurrently with a single program to perform tasks in parallel
    ### Thread safety 
    - A pieces of code is thread-safe if it function correctly when accessed by multiple threads simultaneously without causing data inconsistency or unexpected behavior.
    ### Race condition 
    - A situation where two or more threads are access shared data at the same time and the final result depends on the timing of their execution leading to unpredicted outcome.
    ### Synchronization
    - A mechanism to control access to a shared resources by multiple threads - ensuring that only one thread can access the critical section at a time, thus preventing race condition 

    # States of a Thread (Thread lifecycle)
    ### 1. New - thread is created but not yet started
    ```
    Thread t = new Thread ();
    ``` 
    ### 2. Runnable 
    - thread has been started using start() and is ready to run or is running.
    - it is up to the OS thread scheduler to assign CPU time. 
    ```
    t.start()       // move to runnable
    ```

    ### 3. Blocked 
    - The thread is waiting to acquire a lock on an object (e.g inside synchronized block/method).
    - only one thread can hold the lock; other get block.

    ```
    Synchronized (obj){
        //if another thread already holds on a lock on Obj, the current thread becomes Blocked.
    }  
    ```
    ### 4. Waiting/ Timed_Waiting 
    - These threads are temporarily paused, waiting for a specific condition:

        #### a. Waiting 
        - Waiting indefinitely (unlimited time) for another thread to notify it.
        - used with methods like wait(), join () (without timeout), or park().

        #### b. Timed_Waiting 
        - waiting for limited amount of time.
        - used with sleep (ms), join(ms), or wait (ms).

    ### 5. Terminated (AKA Dead)
    - The thread has finished execution (either normally or due to an exception).
    - can not be restarted. 

    <img alt = "Thread lifecycle" src = "https://www.scientecheasy.com/wp-content/uploads/2020/06/thread-life-cycle.png" width = "400">

# Optional
- Is a **container object** (is an object that stores another object) which may or may not contain a non-null value.
- Introduced in java 8 to handle nullable values and avoid nullPointerException.
- Instead of returning null, methods can return an optional that either contains a non-null value or is empty. 
- make a code more readable and encourages functional programming. 
    ### key methods of optional 
    - isPresent() - checks if the value is present. 
    - ifPresent (consumer) - 
    - orElse (defaultValue) -  returns a value if present, otherwise a default.
    - orElseThrow() - returns a value if present, otherwise throw an exception.
    - Optional.of(value) -  Creates an Optional with a non-null value.
    - Optional.ofNullable(value) - Creates an Optional, allowing null value.
    - Optional.empty () - returns an empty optional

# Functional Interface
- functional interface (introduced on java 8) is an interface that has only one abstract method.
- it can contain multiple default or static methods 
- we use @FunctionalInterface Annotation on the top of the interface. (optional but recommended)
- java provides predefined functional interface in java.util.function package.
    1. predicate <T> - returns true or false.
    2. Function <T,R> - Takes T and returns R
    3. Consumer <T> - consumes input, no return
    4. Supplier <T> - Returns a value.
- Java’s predefined functional interfaces like Function, Consumer, Predicate, etc., were intentionally designed to match common patterns in utility classes, so you can easily plug in method references and lambda expressions without needing to write custom interfaces every time.

    #### Example 
    ```
    @FunctionalInterface
    public interface Function<T, R> {
        R apply(T t);
    }

    Function<String, String> toUpper = String::toUpperCase;

    // Create a function named toUpper that takes a String and returns its uppercase version — by referencing the toUpperCase() method of String.
    ``` 

    ### lambda expression  
    - the shortest way of writing a Anonymous function (a method with out a name).
    - it's introduced on java 8.
    - to use a lambda expression in Java, the target type must be a functional interface. Why?
        - Because a lambda doesn't have a name or its own method body — it needs a "blueprint" to match, and a functional interface provides that.

        ```
        Syntax 
        (parameters) -> expression
            OR
        (parameters) -> {statements}
        ```
        #### Example
    ```
    @FunctionalInterface
    public Interface Runnable {
        void run () {
            System.out.println("Running without lambda");
        }
    }

    public static void main (String [] args){

        Runnable r1 = new Runnable () {

            void run () {
                System.out.println("Running without lambda")
            }

        }

        Runnable r2 = () -> System.out.println("Running with lambda!");

        new Thread (r1).start();
        new Thread (r2).start();

    }
    ``` 
    ### method References
    - it is the shorthand/concise way to write lambda expression that call an existing method. 
        ### Basic Syntax:
        1. ClassName::StaticMethod
        2. ObjectRef::instanceMethod
        3. ClassName::instanceMethod
        4. className::new

    ### Example:
    Using lambda expression 
    ```
    public class Main {
        public static void main(String[] args) {
            List<String> friends = List.of("Biruk", "John", "Abraham");

            List<String> uFriends = friends.stream()
                            .map(friend -> friend.toUpperCase())
                            .toList();

            uFriends.forEach(friend -> System.out.println(friend));

        }
    }
    ```

    Using method references 

    ```
    public class Main {
        public static void main(String[] args) {
            List<String> friends = List.of("Biruk", "John", "Abraham");

            List<String> uFriends = friends.stream()
                            .map(String::toUpperCase)
                            .toList();

            uFriends.forEach(System.out::println);

        }
    }
    ```
    ### Note:
    - Here the class, String and System.out are not functional interfaces.
    - we are referring their methods to match the signature of functional interface.
    - under the hood we have used Function <>



# Annotation 
- Annotation is a form of metadata that provides additional information about the program but does not affect the programs actual execution.
- Annotations are used to give instruction to the compiler, provides runtime information, or configure frameworks. 

# Streams
- streams are also introduced in java 8.
- is a pipeline of data used to processing collections in a functional and declarative way.
- they allow operations like filtering, mapping, and reducing without modifying the original data. 
- streams do not mutate the original list/collection.
- supports lazy evaluation (intermediate operations are executed only when terminal operation is called).
- supports parallel execution (.parallelStream()) for better performance. 
- streams do not store data, they process data step by step and return new streams until the final result is produced by a terminal operation. 

## Intermediate Vs Terminal Operations in Java
- Intermediate
    - Transformations that return another stream (filter(), map(), sorted (), distinct())
    - Lazy execution - doesn't execute immediately, executes only when terminal operation is called.
    - can be chained together.  

- Terminal 
    - produces a final result or a side effect and ends a stream pipe line (collect(), forEach(), reduce(), count())
    - Eager execution - executes immediately, triggers execution of the stream.
    - If there’s no terminal operation, the stream is never executed!

## stream methods 
- some of the commonly used methods includes:

| no| Intermediate method | Purpose                                      |
|---|---------------------|----------------------------------------------|
| 1 | map ()              | Transforms elements                          |
| 2 | filter ()           | Filters elements based on a condition        |
| 3 | distinct ()         | Removes duplicates                           |
| 4 | sorted ()           | Sorts elements                               |
| 5 | limited ()          | Limits the number of elements                |
| 6 | skip ()             |                                              |
| 7 | reduce ()           |                                              |
| 8 | forEach ()          |  Iterates over elements                      |
| 9 | collect ()          |  Converts stream to a List                   |
| 10| count ()            |  Counts elements                             |
| 11| anyMatch ()         |  Checks if any element matches a condition   |
| 12| findFirst ()        |  Returns the first element                   |

<img src= "./Stream-API.png" alt = "Stream API Methods" width = "400">

### Parallel Streams
- creates a parallel stream from a collection.
- the stream operations can be executed concurrently using multiple threads behind the scenes.
- Instead of processing the items one by one (sequentially), it splits the work across multiple threads.
- it attempts to speed things up by using multiple CPU cores.

Example
```
List<String> names = List.of("Biruk", "John", "Abraham");

names.parallelStream()
     .map(name -> name.toUpperCase())
     .forEach(System.out::println);

```
# Reflection API 
- allows us to inspect and manipulate classes, methods, fields and constructors at runtime, even if you don't know their names at compile time.
- have  a key consideration and the downsides while using reflection api includes: 
    - slower performance - reflection is slower than direct method calls. 
    - security risks - bypasses encapsulation, may expose sensitive data.
    - breaks compile-time safety - errors may occur at runtime instead of compile-time. 

### Why we use reflection API?
- Dynamically load classes 
- Inspect class structure (using get methods)
- modify private fields and methods (bypass encapsulation).
- create objects dynamically (without new keyword)

### Key methods in reflection API 
- The java.lang.reflect package provides the Reflection API.
    1. Class <?> - represents a class
    2. Method - represents a method 
    3. Field - represents a field 
    4. Constructor - represents a constructor 

### where is reflection used?
1. Spring framework - dependency Injection 
2. Hibernate - object relational mapping framework
3. Junit - unit testing (invokes test methods dynamically)
4. Serialization and Deserialization 







