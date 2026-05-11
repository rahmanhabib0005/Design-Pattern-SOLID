# Design-Pattern-SOLID

Design Patterns & SOLID Principles implementation with Java.
This repository contains beginner-to-intermediate level examples of SOLID principles and commonly used Design Patterns with simple and understandable implementations.

---

# SOLID Principles

SOLID principles are five object-oriented design principles that help developers write clean, maintainable, scalable, and loosely coupled software.

---

# 1. SRP - Single Responsibility Principle

### Definition

A class should have only one reason to change.
Each class should handle only one responsibility.

### Purpose

* Improves maintainability
* Reduces coupling
* Makes code easier to test and extend

### Real-World Example

* `UserRepository` handles database operations
* `UserReport` handles report generation

```java
// 1. Single Responsibility Principle
class User { String name; }
class UserRepository {
    void save(User user){ System.out.println("Save User "+user.name); }
}
class UserReport {
    void generate(User user){ System.out.println("Generate User "+user.name+" Report."); }
}
class Main {
    public static void main(String[] args) {
        User user = new User();
        user.name = "Habib";
        UserReport userreport = new UserReport();
        userreport.generate(user);
        UserRepository userrepository = new UserRepository();
        userrepository.save(user);
        System.out.println("Start small. Ship something.");
    }
}
```

---

# 2. OCP - Open Closed Principle

### Definition

Software entities should be open for extension but closed for modification.

### Purpose

* New functionality can be added without changing existing code
* Reduces risk of breaking old functionality

### Real-World Example

Adding a new payment method without changing existing payment classes.

```java
// 2. Open-Close Principle
interface PaymentType{ void pay(double amount); }
class CreditCardPayment implements PaymentType {
    @Override
    public void pay(double amount) { System.out.println("Paid " + amount + " using Credit Card"); }
}
class PayPalPayment implements PaymentType {
    @Override
    public void pay(double amount) { System.out.println("Paid " + amount + " using PayPal"); }
}
class PaymentProcessor {
    private PaymentType paymentType;
    public PaymentProcessor(String paymentType) {
        if(paymentType.equalsIgnoreCase("creditcard")) this.paymentType = new CreditCardPayment();
        else if(paymentType.equalsIgnoreCase("paypal")) this.paymentType = new PayPalPayment();
        else throw new IllegalArgumentException("Unsupported payment type");
    }
    public void processPayment(double amount) { paymentType.pay(amount); }
}
public class Main {
    public static void main(String[] args) {
        PaymentProcessor processor1 = new PaymentProcessor("creditcard");
        processor1.processPayment(100.0);
        PaymentProcessor processor2 = new PaymentProcessor("paypal");
        processor2.processPayment(200.0);
    }
}
```

---

# 3. LSP - Liskov Substitution Principle

### Definition

Objects of a superclass should be replaceable with objects of its subclasses without affecting program correctness.

### Purpose

* Ensures safe inheritance
* Prevents unexpected behavior
* Promotes reliable polymorphism

### Real-World Example

Both `FullTimeEmployee` and `PartTimeEmployee` can replace the parent `Employee` type without breaking application behavior.

```java
// 3. Liskov Substitution Principle (LSP)
class Employee {
    public void work() {
        System.out.println("Employee is working");
    }
}
class FullTimeEmployee extends Employee {
    @Override
    public void work() {
        System.out.println("Full Time Employee is working 8 hours");
    }
}
class PartTimeEmployee extends Employee {
    @Override
    public void work() {
        System.out.println("Part Time Employee is working 4 hours");
    }
}
class Main {
    public static void main(String[] args) {
        Employee emp1 = new FullTimeEmployee();
        Employee emp2 = new PartTimeEmployee();
        emp1.work();
        emp2.work();
    }
}
```

---

# 4. ISP - Interface Segregation Principle

### Definition

Clients should not be forced to depend on interfaces they do not use.

### Purpose

* Creates smaller and focused interfaces
* Reduces unnecessary implementation
* Improves maintainability

### Real-World Example

A robot does not need eating functionality, so it should not implement an unnecessary method.

```java
// 4. Interface Segregation Principle (ISP)
interface Workable {
    void work();
}
interface Eatable {
    void eat();
}
class HumanWorker implements Workable, Eatable {
    @Override
    public void work() {
        System.out.println("Human is working");
    }
    @Override
    public void eat() {
        System.out.println("Human is eating");
    }
}
class RobotWorker implements Workable {
    @Override
    public void work() {
        System.out.println("Robot is working");
    }
}
class Main {
    public static void main(String[] args) {
        HumanWorker human = new HumanWorker();
        human.work();
        human.eat();

        RobotWorker robot = new RobotWorker();
        robot.work();
    }
}
```


---

# 5. DIP - Dependency Inversion Principle

### Definition

High-level modules should not depend on low-level modules.
Both should depend on abstractions.

### Purpose

* Reduces tight coupling
* Improves flexibility and testability

### Real-World Example

Notification service depends on the `EmailSender` interface instead of a concrete email implementation.

```java
// 5. Dependency Inversion Principle (DIP)
interface EmailSender { void sendEmail(); }
class EmailService implements EmailSender {
    @Override
    public void sendEmail() { System.out.println("Sending Email"); }
}
class NotificationService {
    private EmailSender emailSender;
    public NotificationService(EmailSender emailSender) { this.emailSender = emailSender; }
    public void notifyUser() { emailSender.sendEmail(); }
}
class Main {
    public static void main(String[] args) {
        EmailSender emailSender = new EmailService();
        NotificationService service = new NotificationService(emailSender);
        service.notifyUser();
    }
}
```

---

# Design Patterns

Design Patterns are reusable solutions to common software design problems.

---

# Singleton Pattern - Creational Pattern

### Definition

Ensures that only one instance of a class exists and provides a global access point to it.

### Common Use Cases

* Database connection
* Logger service
* Configuration manager
* Cache manager

```java
// Singleton Design Pattern
class Singleton{
    private static Singleton instance;
    private Singleton() {}
    public static Singleton getInstance() {
        if (instance == null) { instance = new Singleton();}
        return instance;
    }
    public void showMessage() {System.out.println("Hello World!");}
}

public class Main {
    public static void main(String[] args) {
        Singleton s1 = Singleton.getInstance();
        Singleton s2 = Singleton.getInstance();
        System.out.println(s1 == s2);
        s1.showMessage();
    }
}
```

---

# Factory Pattern - Creational Pattern

### Definition

Creates objects without exposing the object creation logic to the client.

### Purpose

* Encapsulates object creation
* Reduces dependency on concrete classes

### Common Use Cases

* Payment gateway creation
* Notification service generation
* Report generators

```java
// Factory Method Design Pattern
interface Shape {void draw();}
class Circle implements Shape {
    public void draw() {
        System.out.println("Drawing a Circle");
    }
}
class Square implements Shape {
    public void draw() {
        System.out.println("Drawing a Square");
    }
}
class ShapeFactory {
    public Shape getShape(String shapeType) {
        if (shapeType == null) return null;
        if (shapeType.equalsIgnoreCase("CIRCLE")) return new Circle();
        if (shapeType.equalsIgnoreCase("SQUARE")) return new Square();
        return null;
    }
}
class FactoryMethodPattern {
    public static void main(String[] args) {
        ShapeFactory shapeFactory = new ShapeFactory();
        Shape shape1 = shapeFactory.getShape("CIRCLE");
        Shape shape2 = shapeFactory.getShape("SQUARE");
        shape1.draw();
        shape2.draw();
    }
}
class Main {
    public static void main(String[] args) {
        FactoryMethodPattern.main(args);
    }
}
```

---

# Decorator Pattern - Structural Pattern

### Definition

Adds new behavior to objects dynamically without modifying existing code.

### Purpose

* Extends functionality at runtime
* Follows Open Closed Principle

### Common Use Cases

* Coffee customization
* Middleware systems
* Authentication layers
* Logging systems

```java
// Decorator Design Pattern
abstract class coffee {
    public abstract String getDescription();
    public abstract double getCost();
}
class SimpleCoffee extends coffee {
    @Override
    public String getDescription() { return "Simple Coffee"; }
    @Override
    public double getCost() { return 2.0; }
}
abstract class CoffeeDecorator extends coffee {
    protected coffee decoratedCoffee;
    public CoffeeDecorator(coffee decoratedCoffee) { this.decoratedCoffee = decoratedCoffee; }
    @Override
    public String getDescription() { return decoratedCoffee.getDescription(); }
    @Override
    public double getCost() { return decoratedCoffee.getCost(); }
}
class MilkDecorator extends CoffeeDecorator {
    public MilkDecorator(coffee decoratedCoffee) { super(decoratedCoffee); }
    @Override
    public String getDescription() { return decoratedCoffee.getDescription() + ", Milk"; }
    @Override
    public double getCost() { return decoratedCoffee.getCost() + 0.5; }
}
class SugarDecorator extends CoffeeDecorator {
    public SugarDecorator(coffee decoratedCoffee) { super(decoratedCoffee); }
    @Override
    public String getDescription() { return decoratedCoffee.getDescription() + ", Sugar"; }
    @Override
    public double getCost() { return decoratedCoffee.getCost() + 0.2; }
}
public class Main {
    public static void main(String[] args) {
        coffee myCoffee = new SimpleCoffee();
        System.out.println(myCoffee.getDescription() + " $" + myCoffee.getCost());
        myCoffee = new MilkDecorator(myCoffee);
        System.out.println(myCoffee.getDescription() + " $" + myCoffee.getCost());
        myCoffee = new SugarDecorator(myCoffee);
        System.out.println(myCoffee.getDescription() + " $" + myCoffee.getCost());
    }
}
```

---

# Observer Pattern - Behavioral Pattern

### Definition

Defines a one-to-many dependency between objects so that when one object changes state, all dependent objects are notified automatically.

### Purpose

* Supports event-driven systems
* Reduces tight coupling between objects

### Common Use Cases

* Notification systems
* Event listeners
* Real-time applications
* Publish-subscribe systems

```java
// Observer Design Pattern
import java.util.ArrayList;
import java.util.List;
interface Observer { void update(String message); }
class Subject {
    private List<Observer> observers = new ArrayList<>();
    public void attach(Observer observer) { observers.add(observer); }
    public void detach(Observer observer) { observers.remove(observer); }
    public void notifyObservers(String message) {
        for (Observer observer : observers) { observer.update(message); }
    }
}
class ConcreteObserver implements Observer {
    private String name;
    public ConcreteObserver(String name) { this.name = name; }
    @Override
    public void update(String message) {
        System.out.println(name + " received: " + message);
    }
}
public class Main {
    public static void main(String[] args) {
        Subject subject = new Subject();
        Observer observer1 = new ConcreteObserver("Observer 1");
        Observer observer2 = new ConcreteObserver("Observer 2");
        subject.attach(observer1);
        subject.attach(observer2);
        subject.notifyObservers("Hello Observers!");
        subject.detach(observer1);
        subject.notifyObservers("Hello Observers2!");
    }
}
```

---

# Repository Goals

* Learn SOLID principles
* Understand core Design Patterns
* Improve object-oriented programming skills
* Write clean and maintainable code
* Prepare for software engineering interviews

---

# Technologies Used

* Java
* Object-Oriented Programming (OOP)
* SOLID Principles
* Design Patterns

---

# Author

## Habibur Rahman
