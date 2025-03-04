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



