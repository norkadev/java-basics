# Java  Basics
 Basic java concepts with examples.
 

 # SOLID Principle
The SOLID principles are a set of five design principles intended to make software designs more understandable, flexible, and maintainable. Here's an explanation of each principle with Java examples:


**1. Single Responsibility Principle (SRP) A class should have only one reason to change.**

Bad example:
```java
class User {
    public void createUser(String name, String email) {
        // Logic to create a user
    }
    public void sendEmail(String email, String message) {
        // Logic to send an email
    }
}
```
Good example:
```java
class User {
    public void createUser(String name, String email) {
        // Logic to create a user
    }
}
class EmailService {
    public void sendEmail(String email, String message) {
        // Logic to send an email
    }
}
```
In the improved example, User is only responsible for user creation, and EmailService is responsible for sending emails.

**2.Open/Closed Principle (OCP) Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification.**

Bad example:
```java
class Rectangle {
    public double width;
    public double height;
}
class Circle {
    public double radius;
}
class AreaCalculator {
    public double calculateArea(Object shape) {
        if (shape instanceof Rectangle) {
            Rectangle rectangle = (Rectangle) shape;
            return rectangle.width * rectangle.height;
        } else if (shape instanceof Circle) {
            Circle circle = (Circle) shape;
            return Math.PI * circle.radius * circle.radius;
        }
        return 0;
    }
}
```
Good example:
```java
interface Shape {
    double calculateArea();
}
class Rectangle implements Shape {
    public double width;
    public double height;
   
    @Override
    public double calculateArea() {
        return width * height;
    }
}
class Circle implements Shape {
    public double radius;

    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}
class AreaCalculator {
    public double calculateArea(Shape shape) {
        return shape.calculateArea();
    }
}
```
The AreaCalculator now depends on an abstraction (Shape) and can work with any class that implements the Shape interface without modification.

**3.Liskov Substitution Principle (LSP) Subtypes must be substitutable for their base types without altering the correctness of the program.**

Bad example:
```java
class Bird {
    public void fly() {
        System.out.println("Bird is flying");
    }
}
class Ostrich extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Ostrich cannot fly");
    }
}
```
Good example:
```java
class Bird {
    public void eat() {
        System.out.println("Bird is eating");
    }
}
class FlyingBird extends Bird {
    public void fly() {
        System.out.println("Bird is flying");
    }
}
class Ostrich extends Bird {
    // Ostriches eat, but they don't fly
}
```
Instead of forcing Ostrich to implement a fly method that it cannot fulfill, the hierarchy is adjusted to separate flying and non-flying birds.

**4.Interface Segregation Principle (ISP) A client should not be forced to depend on methods it does not use.**
Bad example:
```java
interface Worker {
    void work();
    void eat();
}
class Human implements Worker {
    @Override
    public void work() {
        System.out.println("Human is working");
    }

    @Override
    public void eat() {
        System.out.println("Human is eating");
    }
}
class Robot implements Worker {
    @Override
    public void work() {
        System.out.println("Robot is working");
    }

    @Override
    public void eat() {
        // Robots don't eat, but it's forced to implement this
        throw new UnsupportedOperationException("Robot cannot eat");
    }
}
```
Good example:
```java
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

class Human implements Workable, Eatable {
    @Override
    public void work() {
        System.out.println("Human is working");
    }

    @Override
    public void eat() {
        System.out.println("Human is eating");
    }
}
class Robot implements Workable {
    @Override
    public void work() {
        System.out.println("Robot is working");
    }
}
```
The Worker interface is split into Workable and Eatable, allowing Robot to implement only the Workable interface.

**5.Dependency Inversion Principle (DIP) High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details. Details should depend on abstractions.**

Bad example:
```java
class LightBulb {
    public void turnOn() {
        System.out.println("LightBulb is on");
    }

    public void turnOff() {
        System.out.println("LightBulb is off");
    }
}
class Switch {
    private LightBulb bulb;

    public Switch(LightBulb bulb) {
        this.bulb = bulb;
    }

    public void operate() {
        bulb.turnOn();
    }
}
```
Good example:
```java
interface Switchable {
    void turnOn();
    void turnOff();
}
class LightBulb implements Switchable {
    @Override
    public void turnOn() {
        System.out.println("LightBulb is on");
    }

    @Override
    public void turnOff() {
        System.out.println("LightBulb is off");
    }
}
class Switch {
    private Switchable device;

    public Switch(Switchable device) {
        this.device = device;
    }

    public void operate() {
        device.turnOn();
    }
}
```
The Switch class depends on the Switchable abstraction, not the concrete LightBulb class.

# Order of execution of Initialization blocks and Constructors in Java.
In Java, the order of execution for initialization blocks and constructors is as follows:

1. **Static Initialization Blocks:** These are executed only once when the class is first loaded into memory. They are used to initialize static variables.
2. **Instance Initialization Blocks:** These are executed every time an object of the class is created, before the constructor.
3. **Constructors**: These are executed after the instance initialization blocks, and they are used to initialize the object's state.
Here's an example to illustrate this:
 ```java
class MyClass {
    // Static initialization block
    static {
        System.out.println("Static initialization block");
        staticVariable = 10;
    }

    // Instance initialization block
    {
        System.out.println("Instance initialization block");
        instanceVariable = 20;
    }

    // Static variable
    static int staticVariable;

    // Instance variable
    int instanceVariable;

    // Constructor
    public MyClass() {
        System.out.println("Constructor");
    }

    public static void main(String[] args) {
        System.out.println("Main method");
        MyClass obj1 = new MyClass();
        MyClass obj2 = new MyClass();
    }
}
```
Output:
```java
Main method
Static initialization block
Instance initialization block
Constructor
Instance initialization block
Constructor
```
Explanation:
The main method starts execution.
The static initialization block is executed only once when MyClass is first loaded.
When new MyClass() is called for obj1, the instance initialization block is executed, followed by the constructor.
When new MyClass() is called for obj2, the instance initialization block is executed again, followed by the constructor. The static initialization block is not executed again because it has already been executed when the class was first loaded

# Differences between Lambda Expressions and Closures in Java.
Lambda expressions and closures are related concepts, but they have some key differences in Java.

**Lambda Expressions**
**Definition:** A lambda expression is a concise way to represent an anonymous function. It's essentially a shorthand for creating an instance of a functional interface.
Syntax: ```(parameters) -> expression``` or ```(parameters) -> { statements; }```

**Usage:** Lambda expressions are used to provide the implementation for functional interfaces (interfaces with a single abstract method).

**Scope:** Lambda expressions can access variables from the enclosing scope. However, they can only access final or effectively final variables (variables whose values do not change after initialization).
Example:
```java
interface MyFunctionalInterface {
    int operation(int a, int b);
}

public class Main {
    public static void main(String[] args) {
        // Lambda expression to add two numbers
        MyFunctionalInterface addition = (a, b) -> a + b;
        System.out.println(addition.operation(5, 3)); // Output: 8

        int factor = 2; // Effectively final variable
        // Lambda expression to multiply a number by a factor
        MyFunctionalInterface multiplication = (a, b) -> a * b * factor;
        System.out.println(multiplication.operation(5, 3)); // Output: 30
    }
}
```
**Closures**
**Definition:** A closure is a function that captures variables from its surrounding scope and "closes over" them, allowing the function to access and manipulate those variables even after the outer scope has exited.
**Java's Approximation:** Java's lambda expressions provide a limited form of closure. They can capture variables from the enclosing scope, but with the restriction that these variables must be final or effectively final.
**Mutability:** In a true closure (like in JavaScript or Python), the captured variables can be modified within the closure, and these changes are reflected in the outer scope. Java does not allow this; it only allows access to effectively final variables, preventing modification.
Example demonstrating the difference:
```
public class ClosureExample {
    public static void main(String[] args) {
        int number = 10;

        // Lambda expression capturing 'number' (must be effectively final)
        Runnable incrementer = () -> {
            // number++; // This would cause a compilation error
            System.out.println("Number: " + number);
        };

        incrementer.run(); // Output: Number: 10
    }
}
```
# Final and Effectively Final variables
A **final** variable in Java is a variable whose value cannot be changed once it has been assigned. An **effectively final** variable is a variable that is not explicitly declared as final, but its value is never changed after initialization. **Lambda expressions** and anonymous classes can only access final or effectively final local variables from their enclosing scope.

A **final variable** must be initialized when it is declared or within the constructor of the class. Once initialized, its value cannot be changed.
Example
```
class FinalExample {
    private final int value; // Declared as final

    public FinalExample(int value) {
        this.value = value; // Initialized in the constructor
    }

    public int getValue() {
        return value;
    }

    public static void main(String[] args) {
        FinalExample obj = new FinalExample(10);
        System.out.println(obj.getValue()); // Output: 10

        // obj.value = 20; // This would cause a compilation error because value is final
    }
}
```
# Enclosing Scope
The enclosing scope refers to the region of code where a variable or expression is defined. It determines the visibility and accessibility of variables within that region. In the context of lambda expressions and anonymous classes in Java, the enclosing scope is particularly important because it defines which variables can be accessed from within the lambda or anonymous class.


Types of Enclosing Scopes
**Class Scope:** Variables declared within a class (fields) have class scope. They are accessible to all methods and blocks within the class.

**Method Scope:** Variables declared within a method have method scope. They are accessible only within that method.

**Block Scope:** Variables declared within a block (e.g., inside an if statement, for loop, or any {}) have block scope. They are accessible only within that block.

**Lambda Expression/Anonymous Class Scope:** Lambda expressions and anonymous classes create their own scope, but they can also access variables from their enclosing scope.

# How Enclosing Scope Works with Lambdas and Anonymous Classes
**Demonstrating the Effectively Final Requirement:**
```java
public class EnclosingScopeMutationExample {
    public static void main(String[] args) {
        int counter = 0; // Initially 0

        // Attempting to modify 'counter' inside the lambda
        Runnable incrementer = () -> {
            // counter++; // This will cause a compilation error
            System.out.println("Counter: " + counter);
        };

        // counter++; // This will cause a compilation error because 'counter' is no longer effectively final
        incrementer.run();
    }
}
```
**Accessing Class-Level Variables (Fields):**
Class-level variables (fields) can be accessed and modified within lambda expressions without needing to be final or effectively final, because they are not local variables in the enclosing method or block.
```java
public class EnclosingScopeClassExample {
    private int instanceVariable = 30; // Class-level variable

    public void myMethod() {
        Runnable runnable = () -> {
            System.out.println("Instance Variable: " + instanceVariable); // Accessing class-level variable
        };
        runnable.run(); // Output: Instance Variable: 30
    }

    public static void main(String[] args) {
        EnclosingScopeClassExample obj = new EnclosingScopeClassExample();
        obj.myMethod();
    }
}
```
**Summary**
The enclosing scope is the region of code where a variable is defined, determining its visibility.

Lambda expressions and anonymous classes can access variables from their enclosing scope.

Local variables from the enclosing scope must be final or effectively final to be accessed within lambda expressions and anonymous classes.

Class-level variables (fields) can be accessed and modified without being final or effectively final.

# Anonymous Class

An anonymous class in Java is a class that is defined and instantiated in a single statement without being explicitly declared with a name. It's often used to create short, one-off implementations of interfaces or abstract classes. Anonymous classes are useful when you need to override methods of a class or implement an interface quickly, without the overhead of creating a named class.

**Key Characteristics**

No Name: Anonymous classes don't have a name.

Single Use: They are typically used only once in the code where they are defined.

Declaration and Instantiation: They are declared and instantiated at the same time using the new keyword.

Extends or Implements: They either extend an existing class or implement an interface.

Enclosing Scope: They have access to the enclosing scope, similar to lambda expressions, but with slightly different rules.

**Examples**

1. Implementing a Simple Interface
```java
package org.example;

interface Operation {
    int perform(int a, int b);
}
```
```java
package org.example;

public class Main {
    public static void main(String[] args) {
        Solution sol = new Solution();
        int result = sol.calculate(5, 3, new Operation() {
            @Override
            public int perform(int a, int b) {
                return a + b;
            }
        });
        System.out.println("Result: " + result);
    }
}

class Solution {
    public int solution(int N) {
        // some logic here
        return N * 2;
    }

    public int calculate(int a, int b, Operation operation) {
        return operation.perform(a, b);
    }
}
```
2. Extending an Abstract Class
```java
package org.example;

abstract class AbstractCalculator {
    public abstract int calculate(int a, int b);

    public void printResult(int result) {
        System.out.println("Result: " + result);
    }
}
```
```java
package org.example;

public class Main {
    public static void main(String[] args) {
        AbstractCalculator multiplier = new AbstractCalculator() {
            @Override
            public int calculate(int a, int b) {
                return a * b;
            }
        };

        multiplier.printResult(multiplier.calculate(5, 4)); // Output: Result: 20
    }
}
```
# Abstract Class
An abstract class in Java is a class that cannot be instantiated directly, meaning you cannot create objects of an abstract class. It serves as a blueprint for other classes and is designed to be subclassed. Abstract classes can contain both abstract methods (methods without implementation) and concrete methods (methods with implementation).

**Key Characteristics**

**Cannot be Instantiated:** You cannot create an object of an abstract class using the new keyword.

**Abstract Methods:** Abstract classes may contain abstract methods, which are declared without an implementation. Subclasses must provide implementations for these methods unless the subclass is also abstract.

**Concrete Methods:** Abstract classes can also contain concrete methods, which have implementations. These methods are inherited by subclasses and can be overridden.
abstract Keyword: Abstract classes are declared using the abstract keyword.

**Constructor:** Abstract classes can have constructors, which are called when a subclass is instantiated.

**Purpose:** They are used to define a common interface for a set of related classes.

**Example**
```java
abstract class MyAbstractClass {
    // Abstract method (no implementation)
    abstract void myAbstractMethod();

    // Concrete method (with implementation)
    void myConcreteMethod() {
        System.out.println("This is a concrete method.");
    }

    // Constructor
    public MyAbstractClass() {
        System.out.println("Abstract class constructor.");
    }
}

class MyConcreteClass extends MyAbstractClass {
    @Override
    void myAbstractMethod() {
        System.out.println("Implementation of myAbstractMethod in MyConcreteClass.");
    }
}

public class Main {
    public static void main(String[] args) {
        MyConcreteClass obj = new MyConcreteClass();
        obj.myAbstractMethod();   // Output: Implementation of myAbstractMethod in MyConcreteClass.
        obj.myConcreteMethod();   // Output: This is a concrete method.
    }
}
```
# When to use Lambdas and when to use Anonymous Classes?
**Use Lambda Expressions:**

* For implementing functional interfaces.

* For simple, single-method implementations.

* When conciseness and readability are important.
  
**Use Anonymous Classes:**

* For implementing interfaces or extending classes with multiple methods.
  
* When you need to maintain state within the implementation.

* For compatibility with older codebases.
  
In most modern Java development, lambda expressions are generally preferred for simple, single-method implementations due to their conciseness and readability. However, anonymous classes still have their place in more complex scenarios.

# Give the list of Java Object class methods.
The Object class in Java is the root of the class hierarchy. Every class in Java directly or indirectly inherits from the Object class. Here's a list of the commonly used methods in the Object class with examples:

1. equals(Object obj): Compares two objects for equality. The default implementation checks if the two objects are the same instance.

2. hashCode(): Returns a hash code value for the object. It is used by hash-based collections like HashMap and HashSet. If two objects are equal according to the equals(Object) method, then calling the hashCode method on each of the two objects must produce the same integer result.

```java
import java.util.Objects;

class MyClass {
    int value;

    public MyClass(int value) {
        this.value = value;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        MyClass myClass = (MyClass) obj;
        return value == myClass.value;
    }

    @Override
    public int hashCode() {
        return Objects.hash(value);
    }
}

public class Main {
    public static void main(String[] args) {
        MyClass obj1 = new MyClass(5);
        MyClass obj2 = new MyClass(5);

        System.out.println(obj1.hashCode()); // Output: (some hash code)
        System.out.println(obj2.hashCode()); // Output: (same hash code as obj1)
    }
}
```
3. toString(): Returns a string representation of the object. The default implementation returns a string consisting of the class name of which the object is an instance, the at-sign character `@', and the unsigned hexadecimal representation of the hash code of the object.
4. getClass(): Returns the runtime class of the object. This is useful for obtaining information about the object's type at runtime.
5. clone(): Creates and returns a copy of the object. The class must implement the Cloneable interface to use this method.
```java
   class MyClass implements Cloneable {
    int value;

    public MyClass(int value) {
        this.value = value;
    }

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}

public class Main {
    public static void main(String[] args) throws CloneNotSupportedException {
        MyClass obj1 = new MyClass(5);
        MyClass obj2 = (MyClass) obj1.clone();

        System.out.println(obj1.value); // Output: 5
        System.out.println(obj2.value); // Output: 5
    }
}
```
6. finalize(): Called by the garbage collector when it determines that there are no more references to the object. It is used to perform cleanup operations. Note: This method is discouraged and can be unreliable.
```java
   class MyClass {
    @Override
    protected void finalize() throws Throwable {
        System.out.println("Finalize method called");
    }
}

public class Main {
    public static void main(String[] args) {
        MyClass obj = new MyClass();
        obj = null; // Make the object eligible for garbage collection
        System.gc(); // Suggest garbage collection (not guaranteed)
    }
}
```
7. notify(), notifyAll(), wait(): These methods are used for thread synchronization. wait() causes the current thread to wait until another thread invokes notify() or notifyAll() for the object. notify() wakes up a single thread that is waiting on the object's monitor. notifyAll() wakes up all threads that are waiting on the object's monitor.
```java
   public class Main {
    public static void main(String[] args) {
        Object lock = new Object();

        Thread t1 = new Thread(() -> {
            synchronized (lock) {
                try {
                    System.out.println("Thread 1: Waiting");
                    lock.wait();
                    System.out.println("Thread 1: Resumed");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        Thread t2 = new Thread(() -> {
            synchronized (lock) {
                System.out.println("Thread 2: Notifying");
                lock.notify();
            }
        });

        t1.start();
        try {
            Thread.sleep(1000); // Ensure t1 waits first
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        t2.start();
    }
}
```
Output
```
Thread 1: Waiting
Thread 2: Notifying
Thread 1: Resumed
```

# What is the difference between atomic, volatile, synchronized?
**volatile**
Purpose: Ensures visibility of changes to variables across threads.
Mechanism: When a variable is declared volatile, the Java Memory Model (JMM) ensures that any write to the variable by one thread is immediately visible to other threads.

Use Case: Useful when you need to ensure that multiple threads see the most up-to-date value of a variable without using locks.

Limitations:

Does not provide atomicity for compound operations (e.g., incrementing a variable).

Only guarantees visibility, not mutual exclusion.
```java
class VolatileExample {
    private volatile boolean running = true;

    public void stop() {
        running = false;
    }

    public void run() {
        while (running) {
            // Do something
        }
        System.out.println("Thread stopped");
    }

    public static void main(String[] args) throws InterruptedException {
        VolatileExample example = new VolatileExample();
        Thread thread = new Thread(example::run);
        thread.start();

        Thread.sleep(100);
        example.stop(); // This will eventually stop the thread
        thread.join();
    }
}
```
**synchronized**
Purpose: Provides both mutual exclusion (atomicity) and visibility.

Mechanism: When a method or block of code is synchronized, only one thread can execute that code at a time. It uses locks to achieve this. When a thread enters a synchronized block, it acquires a lock, and when it exits, it releases the lock.

Use Case: Used to protect critical sections of code where multiple threads might access shared resources.

Limitations:
* Can lead to performance overhead due to lock contention.
* Can cause deadlocks if not used carefully.
  
Example:
```java
class SynchronizedExample {
    private int count = 0;

    public synchronized void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }

    public static void main(String[] args) throws InterruptedException {
        SynchronizedExample example = new SynchronizedExample();

        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                example.increment();
            }
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                example.increment();
            }
        });

        t1.start();
        t2.start();

        t1.join();
        t2.join();

        System.out.println("Count: " + example.getCount()); // Output: Count: 2000
    }
}
```
