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
1. **[Observer](#observer)** Allows a subject to notify multiple observers about state changes.
2. **[Strategy](#strategy)** Enables selecting an algorithm's behavior at runtime.
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
The **Facade** pattern is a *structural* design pattern that provides a simplified interface to a complex subsystem. It acts as a "front-facing" API that hides the complexities of the underlying components, making the system easier to use for clients.

In other words, the Facade acts like a wrapper that orchestrates multiple subsystems and exposes a unified, clean interface.

### 🧠 Real-World Analogy
Think of a universal remote control (the facade). Instead of using separate remotes for your TV, sound system, and media player, the universal remote provides one simple interface to control them all, without needing to understand the internal workings of each device.

### 🧪 TypeScript Example
Imagine you’re building a home theater system. The user just wants a method like `watchMovie()`, but behind the scenes, multiple components need to work together.

```ts
// Subsystems
class DVDPlayer {
  on() { console.log("DVD Player ON"); }
  play(movie: string) { console.log(`Playing movie: ${movie}`); }
}

class Projector {
  on() { console.log("Projector ON"); }
  setInput(source: string) { console.log(`Projector input set to: ${source}`); }
}

class SoundSystem {
  on() { console.log("Sound System ON"); }
  setVolume(level: number) { console.log(`Volume set to: ${level}`); }
}

// Facade
class HomeTheaterFacade {
  constructor(
    private dvd: DVDPlayer,
    private projector: Projector,
    private sound: SoundSystem
  ) {}

  watchMovie(movie: string) {
    console.log("Get ready to watch a movie...");
    this.projector.on();
    this.projector.setInput("DVD");
    this.sound.on();
    this.sound.setVolume(5);
    this.dvd.on();
    this.dvd.play(movie);
  }
}

// Usage
const theater = new HomeTheaterFacade(
  new DVDPlayer(),
  new Projector(),
  new SoundSystem()
);

theater.watchMovie("Inception");
```

### ✅ Advantages of the Facade Pattern
1. **Simplifies Complex APIs** Makes subsystems easier to use by exposing just the necessary functionality
2. **Decouples Clients from Subsystems** Reduces dependencies on internal implementation details
3. **Improves Maintainability** Changes in the subsystem don’t necessarily impact client code
4. **Better Code Organization** Especially useful when multiple classes need to work together in a predictable sequence

### ❌ When to Avoid It
1. **Over-Simplification** Facades can hide too much, limiting the flexibility for advanced use cases
2. **Extra Layer Overhead** If your system isn’t very complex, adding a facade may add unnecessary complexity
3. **Encourages God Object** A badly designed facade may grow too large and become a monolithic object that violates the Single Responsibility Principle

### 🤔 When to Use It
* You have a complex system with many interacting parts, and clients only need a subset
* You want to create a clean API for external modules while keeping internal details hidden
* You want to organize code in a way that encapsulates related logic
* You need to provide multiple levels of abstraction (e.g., beginner vs. advanced APIs)

### 🧰 Common Uses in Web Development
* **Abstraction over third-party libraries** e.g., creating a wrapper over Stripe or Firebase to avoid coupling your code to their APIs
* **Service layer in applications** exposing simple methods like `createUser()` that internally handle validation, logging, API calls, etc
* **UI components** simplifying interaction between multiple widgets and components

## Proxy
The **Proxy** pattern is a structural design pattern that provides a *surrogate* or *placeholder* for another object to control access to it. Instead of interacting with the original object directly, clients use the proxy, which can add additional behavior before or after delegating the request to the real object.

|Feature|Proxy pattern|
|-------|-------------|
|Category|Structural|
|Main Goal|Control access to another object|
|Key Benefit|Adds functionality like logging, security, lazy loading, caching, etc|
|Trade-Off|Can increase complexity and hide logic flow|
|Good For|Access control, performance tuning, separation of concerns|

### 🧠 Real-World Analogy
Think of a security guard at a vault. The guard (proxy) doesn't let everyone access the vault (real object) directly. Instead, they may check credentials, log access times, and only then grant access.

### 📦 Types of Proxies
1. **Virtual Proxy** Lazy-loads or defers creation of expensive objects
2. **Protection Proxy** Controls access based on permissions
3. **Remote Proxy** Represents an object in a different address space (e.g., over a network)
4. **Logging/Monitoring Proxy** Adds logging, metrics, or auditing to method calls
5. **Caching Proxy** Caches results to avoid repeated expensive operations

### ⚙️ TypeScript Example
```ts
interface Image {
  display(): void;
}

class RealImage implements Image {
  constructor(private filename: string) {
    this.loadFromDisk();
  }

  private loadFromDisk() {
    console.log(`Loading ${this.filename}...`);
  }

  display() {
    console.log(`Displaying ${this.filename}`);
  }
}

// Proxy class
class ProxyImage implements Image {
  private realImage: RealImage | null = null;

  constructor(private filename: string) {}

  display() {
    if (!this.realImage) {
      this.realImage = new RealImage(this.filename); // lazy loading
    }
    this.realImage.display();
  }
}

// Usage
const image = new ProxyImage("photo.jpg");
image.display(); // Loads and displays
image.display(); // Only displays (no load)
```

### ✅ Advantages of Proxy
* **Access Control** You can limit or customize access to sensitive or expensive resources
* **Lazy Initialization** Load heavy resources only when needed
* **Logging and Monitoring** Add behavior like logging without modifying the original class
* **Performance Boost via Caching** Cache results in memory instead of recomputing or refetching

### ❌ When to Avoid It
* **Added Complexity** Introducing a proxy for trivial use cases can overcomplicate the code
* **Indirect Communication** Debugging can become harder due to indirection
* **Tight Coupling** A badly designed proxy can become tightly coupled to the real object’s implementation

### 🤔 When to Use It
* You want to **control access** to an object (authentication, authorization)
* You want to **defer resource-heavy operations** (e.g., database or network)
* You need to **extend behavior** without modifying the original object
* You want to introduce logging, metrics, or monitoring transparently

### 🧰 Common Use Cases in Web Development
* **API Gateways** A backend proxy that validates, throttles, and logs incoming requests.
* **HTTP Interceptors** Proxies for HTTP calls that inject headers, tokens, or handle errors
* **Image Lazy Loading** Avoid loading images until they're in the viewport
* **Memoization Utilities** Functions wrapped in a proxy to cache results

## Composite
The **Composite** pattern is a *structural* design pattern that allows you to compose objects into tree-like structures to represent part-whole hierarchies. It lets clients treat individual objects and compositions of objects uniformly.

In short, both leaf nodes (simple elements) and composite nodes (containers of elements) share the same interface and can be used interchangeably.

| Feature            | Composite Pattern                                        |
| ------------------ | -------------------------------------------------------- |
| **Category**       | Structural                                               |
| **Main Goal**      | Treat individual objects and groups uniformly            |
| **Key Benefit**    | Simplifies code working with recursive structures        |
| **Trade-Off**      | Can complicate structure when tree behavior isn’t needed |
| **Best Use Cases** | UI hierarchies, file systems, nested structures          |


### 🧠 Real-World Analogy
Think of a folder structure in your computer:
* A folder can contain files or other folders
* Whether it’s a file or a folder, you can perform the same operations (like open, delete, move)
The Composite pattern models this type of relationship in code

### 📦 TypeScript Example

```ts
// Common interface
interface Component {
  display(indent?: string): void;
}

// Leaf
class File implements Component {
  constructor(private name: string) {}

  display(indent: string = ''): void {
    console.log(`${indent}- File: ${this.name}`);
  }
}

// Composite
class Folder implements Component {
  private children: Component[] = [];

  constructor(private name: string) {}

  add(child: Component): void {
    this.children.push(child);
  }

  display(indent: string = ''): void {
    console.log(`${indent}+ Folder: ${this.name}`);
    for (const child of this.children) {
      child.display(indent + '  ');
    }
  }
}

// Usage
const root = new Folder("root");
const src = new Folder("src");
const file1 = new File("index.ts");
const file2 = new File("app.ts");

src.add(file1);
root.add(src);
root.add(file2);

root.display();
```

### ✅ Advantages of Composite
* **Uniformity** Treat all components the same way, whether they’re simple or complex
* **Scalability** Easily extend and build hierarchies
* **Flexibility** You can change the structure of components dynamically
* **Simplifies Client Code** Clients don’t need to distinguish between leaf and composite objects

### ❌ When to Avoid It
* **Overhead for Simple Structures** If your structure is flat or you only need leaves, the pattern introduces unnecessary complexity
* **Can Obscure Behavior** It may be hard to understand the flow of operations in deeply nested composites
* **Difficult to Restrict Operations** You may want different behavior for leaves and composites, but the pattern encourages uniform behavior

### 🤔 When to Use It
* You need to model a tree-like structure, such as
    * UI components (buttons, panels, modals, etc.)
    * File systems or menu hierarchies
    * File systems or menu hierarchies
* You want to apply operations across all elements in a structure uniformly

### 🧰 Common Use Cases in Web Development
* **React component trees** Components can contain other components, following a composite structure
* **DOM manipulation libraries** Operations like `.append()`, `.remove()` apply similarly to all nodes
* **Navigation menus** Dropdowns that contain other submenus and items
* **Game engines** Entities composed of child entities

## Bridge
The **Bridge** patten is a *structural* design pattern that’s used to decouple an abstraction from its implementation so that the two can vary independently.

It achieves this by splitting a class into two parts:
* **Abstraction** the high-level control layer
* **Implementation** the low-level operational layer

| Feature         | Bridge Pattern                                                            |
| --------------- | ------------------------------------------------------------------------- |
| **Category**    | Structural                                                                |
| **Main Goal**   | Separate abstraction and implementation for independent evolution         |
| **Key Benefit** | High flexibility, low coupling                                            |
| **Best For**    | Cross-platform UIs, plugins, external integrations, rendering abstraction |
| **Downside**    | More classes and indirection when not needed                              |


### 🧠 Real-World Analogy
Imagine a remote control (abstraction) that works with multiple brands of televisions (implementations).
You don't change the remote’s interface, but you can plug in new TV brands as long as they implement a compatible behavior

The abstraction contains a reference to the implementation, and the client interacts with the abstraction, not knowing (or caring) how it’s implemented.


### 📦 TypeScript Example
```ts
// Implementation interface
interface Device {
  enable(): void;
  disable(): void;
  isEnabled(): boolean;
}

// Concrete implementations
class TV implements Device {
  private on = false;
  enable() { this.on = true; console.log("TV is on"); }
  disable() { this.on = false; console.log("TV is off"); }
  isEnabled() { return this.on; }
}

class Radio implements Device {
  private on = false;
  enable() { this.on = true; console.log("Radio is on"); }
  disable() { this.on = false; console.log("Radio is off"); }
  isEnabled() { return this.on; }
}

// Abstraction
class RemoteControl {
  constructor(protected device: Device) {}

  togglePower() {
    if (this.device.isEnabled()) {
      this.device.disable();
    } else {
      this.device.enable();
    }
  }
}

// Extended abstraction
class AdvancedRemoteControl extends RemoteControl {
  mute() {
    console.log("Muted the device (simulated)");
  }
}

// Usage
const remote = new AdvancedRemoteControl(new TV());
remote.togglePower();
remote.mute();
```

### ✅ Advantages of the Bridge Pattern
* **Decouples Abstraction from Implementation** You can change either side independently
* **Improved Scalability** Adding new abstractions or implementations doesn't require changing existing code
* **Reduces Code Duplication** Especially useful when a class has multiple variants along two or more dimensions
* **Promotes Composition Over Inheritance** More flexible and dynamic

### ❌ When to Avoid It
* **Unnecessary Complexity** If your abstraction-implementation layers are not expected to vary independently, the pattern may be overkill
* **Simple Systems** In straightforward designs, Bridge can obscure what would otherwise be simple inheritance.

### 🤔 When to Use It
* You expect that abstractions and implementations will evolve separately.
* You want to avoid a combinatorial explosion of subclasses (e.g., shape types × rendering APIs).
* You're working with cross-platform applications (e.g., mobile apps with different native APIs).
* You want to follow the Open/Closed Principle—adding new functionality without modifying existing code.

### 🧰 Use Cases in Web Development
* **Theming systems** UI abstractions that work with various themes
* **Rendering engines** Frontend rendering that supports both canvas and WebGL
* **Media players** Abstract player UI that can control various back-end streaming services
* **Framework adapters** Bridging different APIs without rewriting the logic

## Flyweight
The **Flyweight** pattern is a *structural* design pattern that focuses on minimizing memory usage by sharing as much data as possible with similar objects.

Instead of having many objects that each hold identical data, the Flyweight pattern allows you to store shared data externally and reuse it across multiple instances.

| Feature          | Description                                                        |
| ---------------- | ------------------------------------------------------------------ |
| **Pattern Type** | Structural                                                         |
| **Main Goal**    | Reduce memory footprint by sharing common object data              |
| **Ideal For**    | Large-scale, memory-intensive systems with many similar objects    |
| **Downside**     | More complex architecture and separation of shared vs. unique data |


### 🎯 Core Idea
* Intrinsic state: Shared, immutable data stored in the flyweight.
* Extrinsic state: Data that varies per object and is passed in at runtime.

### 📦 TypeScript Example
Imagine rendering thousands of tree icons on a map
```ts
// Shared data (intrinsic)
class TreeType {
  constructor(
    public name: string,
    public color: string,
    public texture: string
  ) {}

  render(x: number, y: number) {
    console.log(`Rendering ${this.name} tree at (${x}, ${y})`);
  }
}

// Flyweight factory
class TreeFactory {
  private static types: Map<string, TreeType> = new Map();

  static getTreeType(name: string, color: string, texture: string): TreeType {
    const key = `${name}_${color}_${texture}`;
    if (!this.types.has(key)) {
      this.types.set(key, new TreeType(name, color, texture));
    }
    return this.types.get(key)!;
  }
}

// Tree object (extrinsic state)
class Tree {
  constructor(
    private x: number,
    private y: number,
    private type: TreeType
  ) {}

  draw() {
    this.type.render(this.x, this.y);
  }
}

// Client usage
const forest: Tree[] = [];
forest.push(new Tree(1, 2, TreeFactory.getTreeType("Oak", "Green", "Rough")));
forest.push(new Tree(3, 4, TreeFactory.getTreeType("Oak", "Green", "Rough")));
forest.push(new Tree(5, 6, TreeFactory.getTreeType("Pine", "DarkGreen", "Smooth")));
```

### ✅ Advantages of the Flyweight Pattern
* **Memory Efficiency** Greatly reduces memory usage by sharing common data.
* **Performance** Improved speed for large-scale systems with many similar objects.
* **Decouples Shared vs. Unique Data** Cleaner separation of data concerns.

### ❌ Disadvantages / When to Avoid
* **Increased Complexity** Separating intrinsic and extrinsic state can make the code harder to understand
* **Thread Safety Concerns** If shared objects are mutable, concurrency issues can arise
* **Limited Use Cases** Only beneficial when dealing with large numbers of objects with repeated data

### 🤔 When to Use It
* Your app creates a very large number of similar objects (e.g., 10,000+).
* Most object data is shared (e.g., icons, styles, fonts, colors).
* You need to optimize memory in a resource-constrained environment (e.g., games, browsers).
* Objects can be separated into shared (intrinsic) and unique (extrinsic) properties

### 🧰 Real-World Use Cases
* **Browser DOM elements** e.g., sharing tag definitions
* **Text rendering systems** sharing character glyphs
* **Map markers or icons**
* **Game development** sharing textures and sprites
* **Font rendering** only one glyph per character type


---------

## Observer
The **Observer** pattern is a *behavioral* design pattern that establishes a one-to-many dependency between objects. When the state of one object (the subject) changes, all its dependents (observers) are automatically notified and updated.

This pattern promotes loose coupling between components, making it easier to scale and maintain complex systems.

| Feature                | Description                                                        |
| ---------------------- | ------------------------------------------------------------------ |
| **Pattern Type**       | Behavioral                                                         |
| **Problem Solved**     | Keeps objects in sync with subject state changes                   |
| **Best For**           | Event-driven systems, reactive programming, real-time updates      |
| **Downsides**          | Debugging difficulty, potential memory leaks, performance overhead |
| **Common Alternative** | Pub/Sub architecture, callbacks, reactive streams                  |


### 📦 TypeScript Example
```ts
// Observer interface
interface Observer {
  update(data: string): void;
}

// Concrete Observer
class EmailSubscriber implements Observer {
  constructor(private name: string) {}
  update(data: string): void {
    console.log(`${this.name} received update: ${data}`);
  }
}

// Subject interface
interface Subject {
  subscribe(observer: Observer): void;
  unsubscribe(observer: Observer): void;
  notifyObservers(): void;
}

// Concrete Subject
class NewsFeed implements Subject {
  private observers: Observer[] = [];
  private latestNews: string = "";

  subscribe(observer: Observer): void {
    this.observers.push(observer);
  }

  unsubscribe(observer: Observer): void {
    this.observers = this.observers.filter(o => o !== observer);
  }

  addNews(news: string) {
    this.latestNews = news;
    this.notifyObservers();
  }

  notifyObservers(): void {
    for (const observer of this.observers) {
      observer.update(this.latestNews);
    }
  }
}

// Usage
const feed = new NewsFeed();
const user1 = new EmailSubscriber("Alice");
const user2 = new EmailSubscriber("Bob");

feed.subscribe(user1);
feed.subscribe(user2);

feed.addNews("Observer Pattern in TypeScript!");
```

### ✅ Advantages of the Observer Pattern
* **Loose Coupling** Observers and Subjects are independent and unaware of each other's implementation details
* **Dynamic Relationships** You can add or remove observers at runtime
* **Scalable Event Handling** Suitable for implementing custom event systems or reactive architectures
* **Broadcast Capability** One update can propagate to many listeners efficiently

### ❌ Disadvantages / When to Avoid
* **Memory Leaks** If observers are not properly unsubscribed, it can lead to memory leaks
* **Complex Debugging** Chains of notifications can make debugging more difficult
* **Performance Overhead** Notifying a large number of observers frequently may degrade performance
* **Hidden Dependencies** Observers reacting to changes might introduce unexpected side effects

### 🧠 When to Use the Observer Pattern
* Changes in one object need to be automatically reflected in others
* You want a flexible, decoupled communication mechanism between components
* You're implementing an event-driven system, such as UI updates, stock tickers, or live chat
* Your architecture benefits from a reactive data flow, such as in Redux, RxJS, or Vue’s reactivity model

### ❌ When to Avoid It
* You have a small or simple application that doesn't require dynamic updates
* You don’t need to maintain consistency between multiple components
* Performance and memory usage are critical, and you're dealing with many observers with high-frequency updates

### 🧰 Real-World Use Cases
* **UI frameworks** e.g., React’s setState or Angular’s EventEmitters
* **Publish/Subscribe systems**
* **Messaging queues**
* **Event listeners in the DOM**
* **Model-View-Controller (MVC)** The View observes changes in the Model

## Strategy
The **Strategy** pattern is a *behavioral* design pattern that enables selecting an algorithm’s behavior at runtime. It defines a family of interchangeable algorithms (strategies), encapsulates each one, and makes them interchangeable without modifying the client that uses them.

This pattern promotes the **Open/Closed Principle** — classes are open for extension but closed for modification

| Feature          | Description                                                    |
| ---------------- | -------------------------------------------------------------- |
| **Pattern Type** | Behavioral                                                     |
| **Intent**       | Select behavior/algorithm at runtime without changing the code |
| **Best For**     | Swappable logic, configurable workflows                        |
| **Downsides**    | More boilerplate and classes                                   |
| **Alternatives** | Simple function injection, conditionals (if limited variation) |


### 📦 TypeScript Example
```ts
// Strategy interface
interface PaymentStrategy {
  pay(amount: number): void;
}

// Concrete strategies
class CreditCardPayment implements PaymentStrategy {
  pay(amount: number): void {
    console.log(`Paid €${amount} with Credit Card.`);
  }
}

class PayPalPayment implements PaymentStrategy {
  pay(amount: number): void {
    console.log(`Paid €${amount} with PayPal.`);
  }
}

// Context
class ShoppingCart {
  private strategy: PaymentStrategy;

  constructor(strategy: PaymentStrategy) {
    this.strategy = strategy;
  }

  setStrategy(strategy: PaymentStrategy) {
    this.strategy = strategy;
  }

  checkout(amount: number) {
    this.strategy.pay(amount);
  }
}

// Usage
const cart = new ShoppingCart(new CreditCardPayment());
cart.checkout(100); // Paid €100 with Credit Card.

cart.setStrategy(new PayPalPayment());
cart.checkout(50);  // Paid €50 with PayPal.
```

### ✅ Advantages of the Strategy Pattern
* **Flexibility** Swap algorithms without changing the context’s code
* **Separation of Concerns** Keeps algorithm logic separated from the object using it
* **Open/Closed Principle** Add new strategies without changing existing code
* **Testability** Easier to unit test individual strategies in isolation

### ❌ Disadvantages / When to Avoid It
* **Increased Complexity** More classes and interfaces can make the code harder to manage
* **Overhead** Might be overkill if only a single algorithm is ever used
* **Client Awareness** The client must know about the different strategies to choose the right one

### 🧠 When to Use the Strategy Pattern
* You have multiple algorithms for a specific task, and you want to switch between them at runtime
* You need to avoid large conditionals (if/else or switch) based on behavior
* You want to isolate and organize related behaviors
* You want to make a class behaviorally configurable

### ❌ When to Avoid It
* Only one algorithm is needed — adding strategy abstraction adds unnecessary complexity
* The behavior never changes at runtime
* You don’t need to encapsulate or switch logic dynamically

### 🧾 Real-World Use Cases
* Payment processing systems with multiple payment methods
* Sorting algorithms that vary based on data type or size
* Compression strategies (ZIP, RAR, etc.)
* Navigation apps that offer walking, driving, or transit routes
* AI behavior in games (aggressive, defensive, passive strategies)

## Command
The **Command** pattern is a *behavioral* design pattern that turns a request into a standalone object containing all information about the request — including the method to call, the method's arguments, and the receiver of the method.

This allows you to:
* Parameterize objects with operations.
* Queue or log requests.
* Support undoable operations.

It encapsulates a request as an object, thereby decoupling the sender from the receiver.

| Feature          | Description                                                      |
| ---------------- | ---------------------------------------------------------------- |
| **Pattern Type** | Behavioral                                                       |
| **Intent**       | Encapsulate a request as an object                               |
| **Best For**     | Queuing, undoable operations, decoupling caller and handler      |
| **Downsides**    | Boilerplate, class explosion                                     |
| **Alternatives** | Callbacks, simple functions, event emitters (for less structure) |


### 📦 TypeScript Example
```ts
// Receiver
class Light {
  turnOn() {
    console.log("Light is ON");
  }

  turnOff() {
    console.log("Light is OFF");
  }
}

// Command interface
interface Command {
  execute(): void;
  undo(): void;
}

// Concrete commands
class LightOnCommand implements Command {
  constructor(private light: Light) {}

  execute() {
    this.light.turnOn();
  }

  undo() {
    this.light.turnOff();
  }
}

class LightOffCommand implements Command {
  constructor(private light: Light) {}

  execute() {
    this.light.turnOff();
  }

  undo() {
    this.light.turnOn();
  }
}

// Invoker
class RemoteControl {
  private command?: Command;

  setCommand(command: Command) {
    this.command = command;
  }

  pressButton() {
    this.command?.execute();
  }

  pressUndo() {
    this.command?.undo();
  }
}

// Usage
const light = new Light();
const lightOn = new LightOnCommand(light);
const remote = new RemoteControl();

remote.setCommand(lightOn);
remote.pressButton(); // Light is ON
remote.pressUndo();   // Light is OFF
```

### ✅ Advantages of the Command Pattern
* **Decoupling** Sender and receiver are fully decoupled
* **Reusability** Commands can be reused and composed
* **Undo/Redo Support** You can keep a history of executed commands
* **Macro Commands** Easily create batches of commands
* **Flexible Execution** Can be queued, scheduled, or persisted

### ❌ Disadvantages / When to Avoid It
* **Overhead** Introduces many small classes or functions for each command
* **Boilerplate** For simple tasks, it adds unnecessary complexity
* **Verbosity** Not ideal when commands are trivial or infrequent

### 🧠 When to Use the Command Pattern
* You need to parameterize objects by an action to perform
* You want to queue, schedule, or log operations
* You need to support undo/redo functionality
* You want to decouple request invocation from handling logic
* You're implementing macro commands or transactional operations

### ❌ When to Avoid It
* There’s only one simple action being executed, and no need for decoupling
* The overhead of creating multiple classes for each command isn’t justified
* Undo/redo or history tracking is not a concern

### 🧾 Real-World Use Cases
* GUI buttons (e.g. Save, Undo, Redo)
* Macro recording (e.g. a text editor or Photoshop)
* Task queues or job scheduling systems
* Remote controls or home automation systems
* Transaction scripts or rollback support

## Chain of Responsibility
The **Chain of Responsibility** is a *behavioral* design pattern that allows an event (or request) to be passed along a chain of potential handlers until one of them handles it. Each handler either processes the request or passes it to the next handler in the chain.

This pattern decouples the sender of a request from its receivers, giving multiple objects a chance to handle the request without the sender needing to know which one will do so.

| Feature           | Description                                     |
| ----------------- | ----------------------------------------------- |
| **Pattern Type**  | Behavioral                                      |
| **Intent**        | Pass request along a chain until it’s handled   |
| **Best For**      | Middleware, logging, validation pipelines       |
| **Advantages**    | Decouples sender/receiver, flexible, extensible |
| **Disadvantages** | Harder to debug, uncertain request handling     |
| **Common In**     | Express.js middleware, GUI event systems        |

### 🧩 Key Concepts
* **Sender** → Initiates the request
* **Handler** → Abstract interface or class that defines how to handle or pass the request
* **ConcreteHandler** → Specific implementations that either handle the request or delegate it
* **Chain** → The linked sequence of handlers

### ✅ Use Cases
* Logging systems (different log levels: debug, info, error, etc.)
* Event handling in GUI frameworks
* Form validation pipelines
* Request processing in middleware (e.g. Express.js)
* Technical support escalation workflows.

### 📦 TypeScript Example
```ts
// Handler Interface
interface Handler {
  setNext(handler: Handler): Handler;
  handle(request: string): void;
}

// Abstract base handler
abstract class AbstractHandler implements Handler {
  private nextHandler?: Handler;

  setNext(handler: Handler): Handler {
    this.nextHandler = handler;
    return handler;
  }

  handle(request: string): void {
    if (this.nextHandler) {
      this.nextHandler.handle(request);
    } else {
      console.log(`No handler could process the request: ${request}`);
    }
  }
}

// Concrete Handlers
class InfoLogger extends AbstractHandler {
  handle(request: string): void {
    if (request === "info") {
      console.log("InfoLogger: Logging info...");
    } else {
      super.handle(request);
    }
  }
}

class ErrorLogger extends AbstractHandler {
  handle(request: string): void {
    if (request === "error") {
      console.log("ErrorLogger: Logging error!");
    } else {
      super.handle(request);
    }
  }
}

class CriticalLogger extends AbstractHandler {
  handle(request: string): void {
    if (request === "critical") {
      console.log("CriticalLogger: Logging CRITICAL error!! 🚨");
    } else {
      super.handle(request);
    }
  }
}

// Usage
const logger = new InfoLogger();
logger
  .setNext(new ErrorLogger())
  .setNext(new CriticalLogger());

logger.handle("info");     // InfoLogger: Logging info...
logger.handle("error");    // ErrorLogger: Logging error!
logger.handle("critical"); // CriticalLogger: Logging CRITICAL error!!
logger.handle("debug");    // No handler could process the request: debug
```
### ✅ Advantages
* **Loose Coupling** Sender doesn't know which handler will handle the request
* **Flexibility** Easily add or remove handlers
* **Responsibility Sharing** Multiple objects can contribute to handling
* **Open/Closed Principle** Easy to extend without modifying existing code

### ❌ Disadvantages
* **Uncertainty** You can’t guarantee a request will be handled
* **Debugging Difficulty** It might be hard to trace how a request flows through the chain
* **Overhead** For simple cases, this can be overkill

### 🔍 When to Use
* You want multiple handlers to process a request, but only one (or none) will
* The set of handlers should be dynamically changeable at runtime
* You want to avoid tight coupling between the sender and receiver of a request
* You’re implementing middleware, validation, logging, or authorization flows

### 🚫 When to Avoid
* A single handler will always process the request — use direct calls instead
* The request needs a mandatory handler — Chain of Responsibility makes this non-obvious
* The logic becomes too complex and hard to follow through multiple handlers

## Mediator
The **Mediator** is a *behavioral* design pattern that encapsulates how a set of objects interact. It promotes loose coupling by preventing direct communication between components — instead, they communicate through a **mediator** object.

Rather than each object referring to others explicitly, they refer only to the mediator, which handles the interactions. This reduces dependencies and makes the system more maintainable.

| Feature                | Description                                                  |
| ---------------------- | ------------------------------------------------------------ |
| **Pattern Type**       | Behavioral                                                   |
| **Intent**             | Centralize complex communication among components            |
| **Best For**           | UI controls, chat rooms, form state management               |
| **Advantages**         | Decouples objects, improves code organization                |
| **Disadvantages**      | Mediator can grow too large, may overcomplicate simple flows |
| **Real World Example** | Air traffic control system                                   |

### 🧩 Key Concepts
* **Colleagues** Components that need to communicate (e.g., UI elements, services)
* **Mediator** Central component that facilitates communication between colleagues
* **Concrete Mediator** A specific implementation of the mediator that defines communication rules

### ✅ Use Cases
* UI component coordination (e.g., enabling/disabling buttons based on form state)
* Chat room systems where users don’t communicate directly
* Air traffic control (aircraft talk to control tower, not to each other)
* Decoupling complex workflows among many objects or modules

### 📦 TypeScript Example
```ts
// Mediator interface
interface Mediator {
  notify(sender: object, event: string): void;
}

// Concrete Mediator
class DialogMediator implements Mediator {
  constructor(
    private button: Button,
    private input: TextInput
  ) {
    this.button.setMediator(this);
    this.input.setMediator(this);
  }

  notify(sender: object, event: string): void {
    if (sender === this.input && event === "inputChanged") {
      this.button.setEnabled(this.input.value.length > 0);
    }
  }
}

// Components (Colleagues)
class Component {
  protected mediator!: Mediator;

  setMediator(mediator: Mediator) {
    this.mediator = mediator;
  }
}

class Button extends Component {
  private enabled = false;

  setEnabled(value: boolean) {
    this.enabled = value;
    console.log("Button is now", value ? "enabled" : "disabled");
  }
}

class TextInput extends Component {
  private _value = "";

  set value(val: string) {
    this._value = val;
    this.mediator.notify(this, "inputChanged");
  }

  get value() {
    return this._value;
  }
}

// Usage
const input = new TextInput();
const button = new Button();
const mediator = new DialogMediator(button, input);

input.value = "";      // Button is now disabled
input.value = "Hello"; // Button is now enabled
```
### ✅ Advantages
* **Decouples components** Each component only knows about the mediator, not the others
* **Improves maintainability** Changes in interaction logic are localized to the mediator
* **Simplifies communication logic** Prevents tangled "spaghetti code" between components.

### ❌ Disadvantages
* **Mediator can become complex** As interactions grow, the mediator might turn into a God object
* **Single point of failure** If the mediator has a bug, it can impact all communication
* **Overhead** Adds indirection that may be unnecessary in simple systems

### 🔍 When to Use
* You have many objects that communicate in complex but well-defined ways
* You want to reduce coupling between components
* You want to centralize control logic that’s currently scattered across many classes

### 🚫 When to Avoid
* Communication logic is trivial or doesn’t justify the added complexity
* You’re introducing a mediator that becomes too large and unmanageable
* You need high performance with minimal indirection

## State
The **State** pattern is a *behavioral* design pattern that allows an object to change its behavior when its internal state changes. From the outside, it appears as if the object has changed its class.

It’s particularly useful for modeling objects that have different modes, phases, or statuses, each with distinct behavior.

| Feature                | Description                                                  |
| ---------------------- | ------------------------------------------------------------ |
| **Pattern Type**       | Behavioral                                                   |
| **Intent**             | Allow an object to alter its behavior when its state changes |
| **Best For**           | Finite state machines, mode switching                        |
| **Advantages**         | Cleaner logic, scalable, maintainable                        |
| **Disadvantages**      | Many small classes, more indirection                         |
| **Real-World Example** | Media players (Play/Pause/Stop), ATMs, Form wizards          |

### 🧠 Key Concepts
* **Context** The main object whose behavior changes depending on its current state
* **State interface** Declares the behavior shared across different states
* **Concrete states** Implement the behavior specific to each state and define when and how to transition to another state

### ✅ Example Use Cases
* Media players (play, pause, stop)
* Document lifecycle (draft, review, published)
* User authentication (logged out, logged in, admin)
* Traffic lights (green, yellow, red)
* UI buttons (enabled, disabled, loading)

### 📦 TypeScript Example
```ts
// State interface
interface State {
  handle(): void;
}

// Context
class Context {
  private state: State;

  constructor(state: State) {
    this.transitionTo(state);
  }

  transitionTo(state: State) {
    console.log(`Transitioning to ${state.constructor.name}`);
    this.state = state;
    this.state['context'] = this; // allow state to call context.transitionTo
  }

  request() {
    this.state.handle();
  }
}

// Concrete States
class ConcreteStateA implements State {
  context!: Context;

  handle() {
    console.log("Handling State A. Switching to B...");
    this.context.transitionTo(new ConcreteStateB());
  }
}

class ConcreteStateB implements State {
  context!: Context;

  handle() {
    console.log("Handling State B. Switching to A...");
    this.context.transitionTo(new ConcreteStateA());
  }
}

// Usage
const context = new Context(new ConcreteStateA());
context.request(); // Handling State A, then transition
context.request(); // Handling State B, then transition
```

### ✅ Advantages
* **Simplifies complex conditionals** Replaces large if-else or switch statements based on internal state
* **Encapsulation of behavior** Each state is self-contained and focused on specific logic
* **Easier to add new states** You can introduce new behavior without touching the context
* **Improves readability** Cleaner separation of logic tied to specific states

### ❌ Disadvantages
* **Can result in many classes** Each state is usually its own class, which might feel like overhead
* **Increased complexity** For simple scenarios, this pattern can be overkill
* **Harder to trace flow** Transitions may become hard to follow if states transition internally

### 🔍 When to Use
* You have an object with behavior that depends on its current state
* You need to avoid bloated conditional logic
* You want to clearly separate behaviors tied to specific states

### 🚫 When to Avoid
* The number of states is very small and unlikely to change
* State-dependent behavior can be handled with simple conditionals
* You don’t want the overhead of managing multiple classes

## Template Method
## Iterator
## Visitor
## Memento
