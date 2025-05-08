# Main design patterns
Design patterns explanation

## Definition
A **design pattern** is a general, reusable solution to a common problem that occurs within a specific context in software design. It's not finished code, but rather a template or blueprint that can be adapted to solve a particular design issue in your program.

### Key points
* **Proven**: Based on best practices developed over time by experienced developers.
* **Reusable**: Helps solve problems that appear again and again in software architecture.
* **Communicative**: Provides a shared language for developers to discuss solutions (e.g., “Let's use the Singleton here.”).

### Creational patterns (focus on object creation)
1. **[Singleton](#singleton)** Ensures a class has only one instance and provides a global point of access to it.
2. **[Factory Method](#factory-method)** Lets a class defer instantiation to subclasses, promoting loose coupling.
3. **[Abstract Factory](#abstract-factory)** Creates families of related or dependent objects without specifying their concrete classes.
4. **Builder** Separates the construction of a complex object from its representation.
5. **Prototype** Creates new objects by cloning an existing object (the prototype).
6. **Dependency Injection (DI)** Involves providing objects that a class needs (its dependencies) from outside rather than creating them internally. This can be done manually or automatically using a container.
### Structural Patterns (deal with object composition)
1. **Adapter** Allows incompatible interfaces to work together.
2. **Decorator** Adds behavior to an object dynamically without altering its structure.
3. **Facade** Provides a simplified interface to a complex subsystem.
4. **Proxy** Acts as a placeholder or surrogate for another object to control access to it.
5. **Composite** Treats individual objects and compositions of objects uniformly.
6. **Bridge** Separates an object’s abstraction from its implementation.
7. **Flyweight** Reduces the cost of creating and manipulating a large number of similar objects.
### Behavioral Patterns (focus on communication between objects)
1. **Observer** Allows a subject to notify multiple observers about state changes.
2. **Strategy** Enables selecting an algorithm's behavior at runtime.
3. **Command** Encapsulates a request as an object, allowing parameterization and queuing.
4. **Chain of Responsibility** Passes a request along a chain of handlers until one handles it.
5. **Mediator** Centralizes complex communication and control logic between objects.
6. **State** Allows an object to change its behavior when its internal state changes.
7. **Template Method** Defines the skeleton of an algorithm in a method, deferring steps to subclasses.
8. **Iterator** Provides a way to access elements of a collection sequentially without exposing its structure.
9. **Visitor** Lets you define new operations on objects without changing their classes.
10. **Memento** Captures and externalizes an object’s internal state without violating encapsulation, allowing the object to be restored to this state later.

## Singleton
The **Singleton** (Creational) is a creational design pattern that ensures a class has only one instance throughout the application and provides a global point of access to that instance.

In TypeScript, it's often implemented using a private constructor and a static method or property to control instance creation.

### ✅ Advantages of Using a Singleton
1. **Single Point of Access (Global State)** Ideal for managing shared resources like configuration objects, logging services, or caching layers.
2. **Lazy Initialization** The instance is only created when needed, saving memory.
3. **Controlled Access** Ensures consistent access and avoids creating multiple instances that could lead to inconsistent state.
4. **Easy to Mock in Tests** In TypeScript, with proper abstraction, singletons can be mocked or stubbed for unit tests.

### 🛠️ When to Use It
1. For services that maintain global configuration or application state.
2. When you want to control access to a resource (e.g., database connection manager).
3. In small apps or libraries where using full dependency injection frameworks would be overkill.

### ⚠️ When to Avoid Singleton
1. **Global State Overuse** Encourages reliance on shared mutable state, which can lead to tight coupling and harder-to-maintain code.
2. **Difficult Testing in Complex Apps** If not carefully abstracted, it can make unit testing harder due to hidden dependencies.
3. **Concurrency Issues** In multi-threaded environments (less common in TypeScript, more in Java/Go/etc.), special care is needed to make singletons thread-safe.
4. **Not a Replacement for Dependency Injection** Using dependency injection is often more flexible and testable.

```ts
class Logger {
  private static instance: Logger;

  private constructor() {
    // Private constructor prevents external instantiation
  }

  static getInstance(): Logger {
    if (!Logger.instance) {
      Logger.instance = new Logger();
    }
    return Logger.instance;
  }

  log(message: string) {
    console.log(`[LOG]: ${message}`);
  }
}

// Usage
const logger = Logger.getInstance();
logger.log('Singleton pattern in action');
const anotherLogger = Logger.getInstance();

console.log(logger === anotherLogger); // true

```
## Factory Method
The **Factory Method** pattern (Creational) defines an interface for creating objects, but it allows subclasses to alter the type of objects that will be created. Essentially, it delegates the instantiation of objects to subclasses, letting the class determine which type of object to instantiate.

This pattern allows the creation of objects to be separated from their usage, providing flexibility in the object creation process.

#### Structure:
* **Product** Defines the interface for the object that is created.
* **ConcreteProduct** Implements the Product interface. This is the specific object created by the `ConcreteCreator`.
* **Creator** Defines the factory method, which returns a `Product` object. The creator may also define a default implementation for the factory method, but it can be overridden by subclasses.
* **ConcreteCreator** Implements the factory method, which instantiates and returns a specific `ConcreteProduct`.

#### How it works
In the `Factory Method` pattern, instead of creating objects directly using constructors (e.g., `new ClassName()`), the client calls a factory method that’s responsible for creating the object. This allows subclasses to define the concrete type of the object they create, making it possible to change the type of object returned at runtime.

### ✅ Advantages of Using Factory Method
1. **Flexibility and Extensibility**
    * The Factory Method provides an easy way to introduce new types of products without modifying the code that uses the factory. Subclasses can create different types of products by overriding the factory method.
    * This promotes adherence to the Open/Closed Principle (part of **SOLID** principles), as the code is open for extension but closed for modification.
2. **Decoupling Object Creation from Usage**
    * The object creation logic is separated from the actual use of the object. Clients can rely on the abstract `Creator` class, and don't need to know how the object is created, leading to better abstraction and lower coupling.
3. **Easier Maintenance**
    * If the object creation process changes, you only need to modify the factory method, rather than changing every instance where the object is created across your codebase.
4. **Better Organization of Code**
    * Centralizes object creation logic in one place (the factory method), which is easier to manage, especially in large applications.
  
### 🛠️ When to Use Factory Method
* **When the exact type of the object cannot be determined until runtime** If the type of object created depends on external factors (such as configuration or user input), the Factory Method provides flexibility to create the appropriate object dynamically.
* **When you want to centralize object creation** If multiple subclasses need to create objects in a consistent way or if there is complex logic involved in creating objects, it’s useful to centralize that logic in the factory method.
* **When you want to delegate the responsibility of creating objects to subclasses** If subclasses need to have the ability to create their own specific products (or extend functionality), a Factory Method is appropriate to achieve that flexibility.

### ⚠️ When to Avoid Factory Method
1. **When you don't need flexibility** If your system only requires a single type of object, creating that object directly (using a constructor) might be simpler and more straightforward. A Factory Method adds unnecessary complexity in such cases.
2. **When you have simple and static object creation** If the object creation process is straightforward and doesn’t involve complex decisions or variations, introducing a Factory Method might add unnecessary abstraction and boilerplate code
3. **When the hierarchy of creators becomes too complex** If the factory method creates products from many different subclasses, the code can become harder to manage. In these situations, the complexity might outweigh the benefits of using this pattern.
4. **When performance is critical** Sometimes the overhead of having multiple classes for each `Creator` (especially if there are many variants) may impact performance, especially if it is called frequently. If you don't need that level of abstraction, simpler solutions might be better.

```ts
// Product Interface
interface Product {
  operation(): string;
}

// ConcreteProduct: Implements the Product interface
class ConcreteProductA implements Product {
  operation(): string {
    return "Operation A";
  }
}

class ConcreteProductB implements Product {
  operation(): string {
    return "Operation B";
  }
}

// Creator: The factory method
abstract class Creator {
  abstract factoryMethod(): Product;

  public operation(): string {
    const product = this.factoryMethod();
    return `Creator: The product says: ${product.operation()}`;
  }
}

// ConcreteCreator: Implements the factory method
class ConcreteCreatorA extends Creator {
  public factoryMethod(): Product {
    return new ConcreteProductA();
  }
}

class ConcreteCreatorB extends Creator {
  public factoryMethod(): Product {
    return new ConcreteProductB();
  }
}

// Client code
const creatorA = new ConcreteCreatorA();
console.log(creatorA.operation());  // Output: Creator: The product says: Operation A

const creatorB = new ConcreteCreatorB();
console.log(creatorB.operation());  // Output: Creator: The product says: Operation B
```

### In synthesis
The Factory Method design pattern is a powerful tool when you need flexibility in object creation or when you want to decouple the object creation process from the main logic of your application. It promotes loose coupling, scalability, and maintainability. However, it should be used judiciously, as it can add complexity and overhead when not necessary. Use it when you foresee a need for varying object creation logic or when different classes may need to create objects of different types.

## Abstract Factory
