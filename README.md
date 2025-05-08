# Main design patterns
Design patterns explanation

## Definition
A **design pattern** is a general, reusable solution to a common problem that occurs within a specific context in software design. It's not finished code, but rather a template or blueprint that can be adapted to solve a particular design issue in your program.

### Key points
* **Proven**: Based on best practices developed over time by experienced developers.
* **Reusable**: Helps solve problems that appear again and again in software architecture.
* **Communicative**: Provides a shared language for developers to discuss solutions (e.g., “Let's use the Singleton here.”).

### Creational patterns (focus on object creation)
1. **[Singleton](#Singleton)** Ensures a class has only one instance and provides a global point of access to it.
2. **[Factory Method](#Factory%20Method)** Lets a class defer instantiation to subclasses, promoting loose coupling.
3. **Abstract Factory** Creates families of related or dependent objects without specifying their concrete classes.
4. **Builder** Separates the construction of a complex object from its representation.
5. **Prototype** Creates new objects by cloning an existing object (the prototype).
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
The **Singleton** is a creational design pattern that ensures a class has only one instance throughout the application and provides a global point of access to that instance.

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
## Factory Method
Lets a class defer instantiation to subclasses, promoting loose coupling.
