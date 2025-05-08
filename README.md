# Main design patterns

A **design pattern** is a general, reusable solution to a common problem that occurs within a specific context in software design. It's not finished code, but rather a template or blueprint that can be adapted to solve a particular design issue in your program.

## Key points
* **Proven**: Based on best practices developed over time by experienced developers.
* **Reusable**: Helps solve problems that appear again and again in software architecture.
* **Communicative**: Provides a shared language for developers to discuss solutions (e.g., “Let's use the Singleton here.”).

## Creational patterns (focus on object creation)
1. **[Singleton](#singleton)** Ensures a class has only one instance and provides a global point of access to it.
2. **[Factory Method](#factory-method)** Lets a class defer instantiation to subclasses, promoting loose coupling.
3. **[Abstract Factory](#abstract-factory)** Creates families of related or dependent objects without specifying their concrete classes.
4. **[Builder](#builder)** Separates the construction of a complex object from its representation.
5. **[Prototype](#prototype)** Creates new objects by cloning an existing object (the prototype).
6. **[Dependency Injection (DI)](#dependency-injection-di)** Involves providing objects that a class needs (its dependencies) from outside rather than creating them internally. This can be done manually or automatically using a container.
## Structural Patterns (deal with object composition)
1. **[Adapter](#adapter)** Allows incompatible interfaces to work together.
2. **[Decorator](#decorator)** Adds behavior to an object dynamically without altering its structure.
3. **[Facade](#facade)** Provides a simplified interface to a complex subsystem.
4. **[Proxy](#proxy)** Acts as a placeholder or surrogate for another object to control access to it.
5. **[Composite](#composite)** Treats individual objects and compositions of objects uniformly.
6. **[Bridge](#bridge)** Separates an object’s abstraction from its implementation.
7. **[Flyweight](#flyweight)** Reduces the cost of creating and manipulating a large number of similar objects.
## Behavioral Patterns (focus on communication between objects)
1. **[Observer](observer)** Allows a subject to notify multiple observers about state changes.
2. **[Strategy](strategy)** Enables selecting an algorithm's behavior at runtime.
3. **[Command](#command)** Encapsulates a request as an object, allowing parameterization and queuing.
4. **[Chain of Responsibility](#chain-of-responsibility)** Passes a request along a chain of handlers until one handles it.
5. **[Mediator](#mediator)** Centralizes complex communication and control logic between objects.
6. **[State](#state)** Allows an object to change its behavior when its internal state changes.
7. **[Template Method](#template-method)** Defines the skeleton of an algorithm in a method, deferring steps to subclasses.
8. **[Iterator](#iterator)** Provides a way to access elements of a collection sequentially without exposing its structure.
9. **[Visitor](#visitor)** Lets you define new operations on objects without changing their classes.
10. **[Memento](#memento)** Captures and externalizes an object’s internal state without violating encapsulation, allowing the object to be restored to this state later.

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

The **Abstract Factory** is a creational design pattern that provides an interface for creating families of related or dependent objects without specifying their concrete classes.

In simpler terms, it’s a pattern that allows you to create multiple objects that are designed to work together, without knowing exactly which classes will be instantiated. It builds on the Factory Method, but instead of just one product, it handles multiple related products.

### ✅ Advantages
* **Encapsulation of object creation** You centralize the logic for instantiating families of related objects.
* **Consistency** You ensure that objects from the same family (e.g., dark theme components) are used together
* **Scalability** It becomes easier to add new families of products without modifying existing code
* **Dependency inversion** The client is decoupled from the specific classes it uses

## ❌ When to Avoid It
* **Unnecessary complexity** If you don’t need to support multiple families of related objects, using this pattern may overcomplicate your code
* **Harder to add new products** If you need to add a new component (e.g., a `Slider`), you must update every concrete factory to implement the new method
* **Too abstract** The level of abstraction can make the code harder to follow for simple use cases

## 🧠 When to Use It
* When your code needs to work with various families of related products (e.g., UI toolkits, database drivers, cross-platform applications)
* When you want to enforce that objects from the same family are used together
* When your application supports multiple themes, modes, or environments

### 🏗 Example Use Case
Imagine you're building a UI library that supports both `light` and `dark` themes. You might have different sets of components (e.g., buttons, text fields) for each theme.

Instead of instantiating each component manually based on the theme, the Abstract Factory lets you define:

```ts
interface UIComponentFactory {
  createButton(): Button;
  createInput(): Input;
}

class LightThemeFactory implements UIComponentFactory {
  createButton() {
    return new LightButton();
  }
  createInput() {
    return new LightInput();
  }
}

class DarkThemeFactory implements UIComponentFactory {
  createButton() {
    return new DarkButton();
  }
  createInput() {
    return new DarkInput();
  }
}
```
Client code uses the factory without knowing which specific implementation is being used:

```ts
function renderUI(factory: UIComponentFactory) {
  const button = factory.createButton();
  const input = factory.createInput();
  // Render the components
}

```

## Builder
The **Builder** is a *creational* design pattern used to construct complex objects step-by-step. Unlike other creational patterns, the Builder allows you to produce different representations of an object using the same construction process.

It’s particularly useful when:
* You have an object with many optional fields or nested structures.
* You want to avoid telescoping constructors (constructors with many parameters).
* You need more control over the object creation process.

### 🏗 Example Use Case
Imagine you're creating a system to build `UserProfile` objects:
```ts
interface UserProfile {
  name: string;
  email?: string;
  address?: string;
  phone?: string;
}
```
Using a builder:
```ts
class UserProfileBuilder {
  private profile: Partial<UserProfile> = {};

  setName(name: string): this {
    this.profile.name = name;
    return this;
  }

  setEmail(email: string): this {
    this.profile.email = email;
    return this;
  }

  setAddress(address: string): this {
    this.profile.address = address;
    return this;
  }

  build(): UserProfile {
    if (!this.profile.name) {
      throw new Error("Name is required");
    }
    return this.profile as UserProfile;
  }
}
```
Usage:

```ts
const user = new UserProfileBuilder()
  .setName("Alice")
  .setEmail("alice@example.com")
  .build();
```
### ✅ Advantages
* **Readable and maintainable** Especially for objects with many optional or nested properties
* **Immutable result** You can create immutable objects in a controlled way
* **Avoids constructor overloads** No need for many constructors with different parameters
* **Flexible creation** Easy to reuse or customize object creation logic

### ❌ When to Avoid It
* **Simple objects** If your object only has a few fields, a simple constructor or object literal is clearer and easier
* **Extra complexity** Introducing builders can be overkill for small applications or data models
* **Verbose code** Requires writing a lot of boilerplate if not using automation or helper libraries

### 🧠 When to Use It
* When constructing an object requires multiple steps or complex configuration
* When many variations of the same object need to be created
* When an object needs to be immutable after construction but is too complex for a constructor alone

## Prototype
The **Prototype** pattern is a *creational* design pattern that enables you to create new objects by copying (cloning) existing ones, rather than creating them from scratch.

The idea is to create a prototypical instance of a class and then duplicate it whenever a new object is needed, instead of instantiating it via constructors. This is particularly useful when object creation is costly or complex.

### 🏗 Example Use Case
Imagine you're developing a system for a game where you have many types of enemies with a lot of shared configuration. Creating each enemy from scratch could be inefficient.
```ts
interface Enemy {
  clone(): Enemy;
}

class Orc implements Enemy {
  constructor(
    public health: number,
    public damage: number,
    public weapon: string
  ) {}

  clone(): Enemy {
    return new Orc(this.health, this.damage, this.weapon);
  }
}
```
Usage:
```ts
const baseOrc = new Orc(100, 15, "Axe");

const fastOrc = baseOrc.clone();
fastOrc.health = 80; // Customize the clone
```

### ✅ Advantages
* **Performance** Cloning objects can be faster than creating them from scratch—especially when construction is expensive (e.g., involves file I/O or database access)
* **Simplifies object creation** Especially when the setup is non-trivial or involves multiple steps
* **Decouples code** You don't need to know the concrete class of the object you're copying
* **Supports runtime configuration** New objects can be created dynamically at runtime without hard-coding their classes

### ❌ When to Avoid It
* **Shallow copies vs. deep copies** If your object contains complex or nested structures, cloning can lead to bugs if references aren't handled properly
* **Maintenance complexity** Managing clone methods across many subclasses can become tedious and error-prone
* **Better alternatives exist** In many modern applications, builders or factories provide more transparent and maintainable solutions

### 🧠 When to Use It
* When object creation is expensive or involves a lot of configuration
* When you want to avoid subclassing and use object composition
* When you want to dynamically add or change configurations at runtime without altering classes
* When you need to keep object creation flexible and decoupled from the code that uses it
* 
## Dependency Injection (DI)
**Dependency Injection (DI)** is a *creational* design pattern (sometimes considered a structural technique as well) that allows a class to receive its dependencies from external sources rather than creating them itself.

In simpler terms: instead of a class building or looking for its collaborators (e.g., services, APIs, database connections), these are “injected” from the outside—usually via the constructor, a method, or a property.

### 🏗 Example in TypeScript
Suppose you have a `UserServic`e that needs a `UserRepository` to fetch user data
```ts
class UserRepository {
  getUser(id: number) {
    return { id, name: "Alice" };
  }
}

class UserService {
  constructor(private userRepository: UserRepository) {}

  getUserProfile(id: number) {
    return this.userRepository.getUser(id);
  }
}
```
Here, `UserRepository` is injected into `UserService`, rather than `UserService` creating its own `UserRepository`. This makes the service more flexible and testable.

### ✅ Advantages
* **Improved Testability** You can easily mock or stub dependencies in unit tests
* **Looser Coupling** Classes depend on abstractions, not concrete implementations, making code more maintainable and interchangeable
* **Greater Reusability and Flexibility** Components can be reused in different contexts by simply injecting different dependencies
* **Easier to Manage Complexity** Especially in large applications, centralizing dependency configuration (e.g., via a DI container) improves structure

### ❌ When to Avoid It
* **Overkill for Small Projects** In small apps, DI can introduce unnecessary complexity without much benefit
* **Indirection Overload** Excessive DI can make code harder to follow—tracing what gets injected where may become confusing
* **Hidden Dependencies** If DI is not clearly documented or structured, developers may lose sight of what a class really needs
* **Performance Considerations** Some DI containers (especially in other languages) can introduce a startup cost, though this is negligible in most frontend TypeScript apps

### 🧠 When to Use It
* When building modular, testable components
* In large-scale applications where dependency management is important
* When applying inversion of control (IoC) principles
* In Angular, which has a built-in DI framework
* In React, often used indirectly via context providers, hooks, or custom dependency injection setups

---------

## Adapter
The **Adapter** pattern is a structural design pattern that allows objects with incompatible interfaces to work together. It acts as a bridge between two objects by wrapping one of them to match the expected interface.

In essence, the Adapter pattern lets you adapt an existing class (or module) to a new interface without modifying the original code.
### 🎯 Real-World Analogy
Imagine a power adapter for a wall socket. Your laptop charger uses a certain plug shape, but the wall socket in a different country uses another. Instead of buying a new charger, you use an adapter to bridge the two.

### 🏗 TypeScript Example
Suppose you have a `LegacyPrinter` with an old method name, and you want to use it in a system that expects a `print()` method:
```ts
// Existing incompatible class
class LegacyPrinter {
  printDocument(doc: string) {
    console.log(`Printing (legacy): ${doc}`);
  }
}

// Expected interface
interface Printer {
  print(doc: string): void;
}

// Adapter that wraps the LegacyPrinter
class LegacyPrinterAdapter implements Printer {
  constructor(private legacyPrinter: LegacyPrinter) {}

  print(doc: string): void {
    this.legacyPrinter.printDocument(doc);
  }
}

// Usage
const legacy = new LegacyPrinter();
const printer: Printer = new LegacyPrinterAdapter(legacy);

printer.print("My Report");
```
### ✅ Advantages of the Adapter Pattern
1. **Reusability** You can reuse existing code (like third-party libraries or legacy systems) without modification
2. **Decoupling** Your main codebase remains decoupled from the specifics of the old or external interface
3. **Improved Flexibility** You can work with multiple implementations of similar functionality by adapting them to a common interface
4. **Single Responsibility Principle** Adapters isolate interface translation logic in a single place

### ❌ When to Avoid It
1. **Overuse Can Lead to Code Bloat** If overused, especially with poorly designed third-party APIs, adapters can become numerous and complex
2. **Hides Bad Design** It may be used to cover up deeper issues in system design instead of refactoring incompatible parts properly
3. **Performance Overhead** There might be a slight overhead in wrapping and delegating calls, though usually negligible

### 🤔 When to Use It
* Integrating a *legacy system* into a new application
* Connecting two incompatible APIs or libraries
* Allowing a system to work with multiple data sources or services without changing business logic
* Adapting third-party code to fit your application’s interface

### 🧰 Common Uses in Web Development
* Wrapping an old REST client to match your modern fetch-based API interface
* Creating adapters to bridge differences between two state management libraries
* Creating React components that adapt data from a GraphQL API into a prop format expected by a legacy component

## Decorator
The **Decorator** pattern is a *structural* design pattern that allows you to dynamically add behavior or responsibilities to an object without altering its structure or modifying its source code.

Decorators wrap the original object inside a special wrapper class that implements the same interface and adds extra behavior **before or after** delegating to the original object.

### 🎯 Real-World Analogy
Think of a coffee shop. You start with a simple coffee (the core object), and then you can decorate it with milk, sugar, caramel, etc. (decorators). Each decorator enhances the coffee without changing the original coffee's structure.

### 🏗 TypeScript Example
Let's model a simple notification system that sends emails. Later, we want to add SMS and push notifications without modifying the original class.
```ts
// Base interface
interface Notifier {
  send(message: string): void;
}

// Concrete implementation
class EmailNotifier implements Notifier {
  send(message: string): void {
    console.log(`Email: ${message}`);
  }
}

// Base decorator
class NotifierDecorator implements Notifier {
  constructor(protected wrappee: Notifier) {}

  send(message: string): void {
    this.wrappee.send(message);
  }
}

// Concrete decorator: SMS
class SMSNotifier extends NotifierDecorator {
  send(message: string): void {
    super.send(message);
    console.log(`SMS: ${message}`);
  }
}

// Concrete decorator: Push
class PushNotifier extends NotifierDecorator {
  send(message: string): void {
    super.send(message);
    console.log(`Push: ${message}`);
  }
}

// Usage
const emailOnly = new EmailNotifier();
const emailAndSms = new SMSNotifier(emailOnly);
const allNotifiers = new PushNotifier(emailAndSms);

allNotifiers.send("Your order has been shipped.");
```
Output:
```ts
Email: Your order has been shipped.
SMS: Your order has been shipped.
Push: Your order has been shipped.
```
### ✅ Advantages of the Decorator Pattern
1. **Open/Closed Principle** You can add new features without altering existing code
2. **Flexible Composition** Behavior can be combined in different ways at runtime
3. **Avoids Class Explosion** Compared to subclassing for every variation, decorators allow modular extension
4. **Reusable Decorators** Each decorator is self-contained and can be reused across the system

### ❌ When to Avoid It
1. **Too Many Small Classes** May result in a large number of decorator classes, making code harder to follow
2. **Complex Debugging** Stacking multiple decorators can complicate traceability and debugging
3. **Alternative: Function Composition** In functional programming (or modern JavaScript/TypeScript), higher-order functions or hooks might be simpler and cleaner

### 🤔 When to Use It
* When you want to add behavior dynamically to objects.
* When subclassing would lead to an explosion of subclasses.
* When you need to combine different responsibilities in flexible ways.
* In UI components (e.g., React HOCs or decorators for styling, analytics, etc.).

## Facade
## Proxy
## Composite
## Bridge
## Flyweight

---------

## Observer
## Strategy
## Command
## Chain of Responsibility
## Mediator
## State
## Template Method
## Iterator
## Visitor
## Memento
