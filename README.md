# java-basics
 Basic java concepts with examples

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

In the improved example, User is only responsible for user creation, and EmailService is responsible for sending emails.

** 2.Open/Closed Principle (OCP) Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification.**

Bad example:

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

Good example:

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

The AreaCalculator now depends on an abstraction (Shape) and can work with any class that implements the Shape interface without modification.

**3.Liskov Substitution Principle (LSP) Subtypes must be substitutable for their base types without altering the correctness of the program.

Bad example:

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

Good example:

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

Instead of forcing Ostrich to implement a fly method that it cannot fulfill, the hierarchy is adjusted to separate flying and non-flying birds.

**4. Interface Segregation Principle (ISP) A client should not be forced to depend on methods it does not use.

Bad example:

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

Good example:

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

The Worker interface is split into Workable and Eatable, allowing Robot to implement only the Workable interface.

**5. Dependency Inversion Principle (DIP) High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details. Details should depend on abstractions.**

Bad example:

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

Good example:

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

The Switch class depends on the Switchable abstraction, not the concrete LightBulb class.
