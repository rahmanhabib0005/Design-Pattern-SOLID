# Design-Pattern-SOLID
Design Pattern &amp; SOLID Principle with Java

# SOLID Principles
# 1. SRP - Single Responsibility Principle
```
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

# 2. OCP - Open Close Principle
```
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

# LSP & ISP - Liskove Subtitution Principle & Interface Segregation Principle
```
// 3. Liskov Substitution Principle (LSP)
// 4. Interface Segregation Principle (ISP) (This example demonstrates both LSP and ISP)
interface Employee { void work(); void eat(); }
interface Travel { void travel(); }
class Developer implements Employee, Travel {
    @Override
    public void work() { System.out.println("Developer is working"); }
    @Override
    public void eat() { System.out.println("Developer is eating"); }
    @Override
    public void travel() { System.out.println("Developer is traveling"); }
}
class RemoteDeveloper implements Employee {
    @Override
    public void work() { System.out.println("Remote Developer is working"); }
    @Override
    public void eat() { System.out.println("Remote Developer is eating"); }
}
class Main {
    public static void main(String[] args) {
        Developer dev = new Developer();
        dev.work(); dev.eat(); dev.travel(); 
        Employee remoteDev = new RemoteDeveloper();
        remoteDev.work(); remoteDev.eat();
    }
}
```
# DIP - Dependency Inversion Principle
```
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


# Design Pattern
# Signleton Pattern - Creational
```
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
# Factory Pattern -- Creational
```
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
# Decorator Pattern -- Structural
```
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
# Observer Pattern - Behavioral
```
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
