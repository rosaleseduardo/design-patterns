<div align = "center">
  <h1> Design Patterns </h1>
</div>

## Table of Contents

- [What's a design pattern?](#whats-a-design-pattern)
- [What does the pattern consist of?](#what-does-the-pattern-consist-of)
- [Why should I learn patterns?](#why-should-i-learn-patterns)
- [Classification of patterns](#classification-of-patterns)

# What's a design pattern?

Design patterns are typical solutions to commonly occurring problems in software
design. They are like pre-made blueprints that you can customize to solve a
recurring design problem in your code.

You can’t just find a pattern and copy it into your program, the way you can with
off-the-shelf functions or libraries. The pattern is not a specific piece of code,
but a general concept for solving a particular problem. You can follow the pattern
details and implement a solution that suits the realities of your own program.

Patterns are often confused with algorithms, because both concepts describe
typical solutions to some known problems. While an algorithm always defines a
clear set of actions that can achieve some goal, a pattern is a more high-level
description of a solution. The code of the same pattern applied to two different
programs may be different.

An analogy to an algorithm is a cooking recipe: both have clear steps to achieve
a goal. On the other hand, a pattern is more like a blueprint: you can see what
the result and its features are, but the exact order of implementation is up to
you.

# What does the pattern consist of?

Most patterns are described very formally so people can reproduce them in many
contexts. Here are the sections that are usually present in a pattern description:

1. **Intent** of the pattern briefly describes both the problem and the solution.
2. **Motivation** further explains the problem and the solution the pattern makes
   possible.
3. **Structure** of classes shows each part of the pattern and how they are related.
4. **Code example** in one of the popular programming languages makes it easier to
   grasp the idea behind the pattern.

Some pattern catalogs list other useful details, such as applicability of the
pattern, implementation steps and relations with other patterns.

# Why should I learn patterns?

The truth is that you might manage to work as a programmer for many years without
knowing about a single pattern. A lot of people do just that. Even in that case,
though, you might be implementing some patterns without even knowing it. So why
would you spend time learning them?

- Design patterns are a toolkit of **tried and tested solutions** to common
  problems in software design. Even if you never encounter these problems,
  knowing patterns is still useful because it teaches you how to solve all
  sorts of problems using principles of object-oriented design.

- Design patterns define a common language that you and your teammates can use
  to communicate more efficiently. You can say, “Oh, just use a Singleton for
  that,” and everyone will understand the idea behind your suggestion. No need
  to explain what a singleton is if you know the pattern and its name.

# Classification of patterns

Design patterns differ by their complexity, level of detail and scale of
applicability to the entire system being designed. I like the analogy to road
construction: you can make an intersection safer by either installing some
traffic lights or building an entire multi-level interchange with underground
passages for pedestrians.

The most basic and low-level patterns are often called idioms. They usually apply
only to a single programming language.

The most universal and high-level patterns are architectural patterns. Developers
can implement these patterns in virtually any language. Unlike other patterns,
they can be used to design the architecture of an entire application.

In addition, all patterns can be categorized by their intent, or purpose. This
book covers three main groups of patterns:

- **Creational patterns** provide object creation mechanisms that increase
  flexibility and reuse of existing code.

- **Structural patterns** explain how to assemble objects and classes into larger
  structures, while keeping these structures flexible and efficient.

- **Behavioral patterns** take care of effective communication and the assignment
  of responsibilities between objects.

# Creational Design Patterns

- [Factory Method](#factory-method)
- [Abstract Factory](#abstract-factory)
- [Builder](#builder)
- [Prototype](#prototype)
- [Singleton](#singleton)

# Structural Design Patterns

- [Adapter](#adapter)
- [Decorator](#decorator)
- [Facade](#facade)

# Behavioral Patterns

- [Observer](#observer)

## Factory Method

### Intent

Factory Method is a creational design pattern that provides a blueprint for creating things. It helps organize how to make objects in a big group. But here's the twist: each specific type of thing in that group can be made in its own unique way by smaller groups within the big group. It's like having a master plan for creating different items with some flexibility for customization.

### Problem

Imagine that you’re creating a logistics management application. The first version of your app can only handle transportation by trucks, so the bulk of your code lives inside the **Truck** class.

After a while, your app becomes pretty popular. Each day you receive dozens of requests from sea transportation companies to incorporate sea logistics into the app.

Great news, right? But how about the code? At present, most of your code is coupled to the **Truck** class. Adding **Ships** into the app would require making changes to the entire codebase. Moreover, if later you decide to add another type of transportation to the app, you will probably need to make all of these changes again.

As a result, you will end up with pretty nasty code, riddled with conditionals that switch the app’s behavior depending on the class of transportation objects.

### Solution

The Factory Method pattern suggests that you replace direct object construction calls (using the **new** operator) with calls to a special _factory_ method. Don’t worry: the objects are still created via the **new** operator, but it’s being called from within the factory method. Objects returned by a factory method are often referred to as _products_.

<div align="center">
  <img src="./assets/factory - solution1.png" alt="Factory Method - Solution 1"/>
  <p> Subclasses can alter the class of objects being returned by the factory method.</p>
</div>

At first glance, this change may look pointless: we just moved the constructor call from one part of the program to another. However, consider this: now you can override the factory method in a subclass and change the class of products being created by the method.

There’s a slight limitation though: subclasses may return different types of products only if these products have a common base class or interface. Also, the factory method in the base class should have its return type declared as this interface.

<div align="center">
  <img src="./assets/factory - solution2.png" alt="Factory Method - Solution 2"/>
  <p>All products must follow the same interface.</p>
</div>

For example, both **Truck** and **Ship** classes should implement the **Transport** interface, which declares a method called **deliver**. Each class implements this method differently: trucks deliver cargo by land, ships deliver cargo by sea. The factory method in the **RoadLogistics** class returns truck objects, whereas the factory method in the **SeaLogistics** class returns ships.

<div align="center">
  <img src="./assets/factory - solution3.png" alt="Factory Method - Solution 3"/>
  <p>As long as all product classes implement a common interface, you can pass their objects to the client code without breaking it.</p>
</div>

The code that uses the factory method (often called the _client_ code) doesn’t see a difference between the actual products returned by various subclasses. The client treats all the products as abstract **Transport**. The client knows that all transport objects are supposed to have the **deliver** method, but exactly how it works isn’t important to the client.

### Structure

<div align="center">
  <img src="./assets/factory - solution4.png" alt="Factory Method - Solution 4"/>
</div>

### Pros

1. You avoid tight coupling between the creator and the concrete products.
2. _Single Responsibility Principle_. You can move the product creation code into one place in the program, making the code easier to support.
3. _Open/Closed Principle_. You can introduce new types of products into the program without breaking existing client code.
4. Polimorfism can be applied.

### Cons

1. The code may become more complicated since you need to introduce a lot of new subclasses to implement the pattern. The best case scenario is when you’re introducing the pattern into an existing hierarchy of creator classes.

## Abstract Factory

### Intent

Abstract Factory is a creational design pattern that defines an interface for creating families of related or dependent objects without specifying their concrete classes. It encapsulates the process of object creation, allowing clients to create objects without knowing their specific types.

### Problem

Imagine that you’re creating a furniture shop simulator. Your code consists of classes that represent:

1. A family of related products, say: **Chair** + **Sofa** + **CoffeeTable**.
2. Several variants of this family. For example, products **Chair** + **Sofa** + **CoffeeTable** are available in these variants: **Modern**, **Victorian**, **ArtDeco**.

<div align="center">
  <img src="./assets/abstract-factory - solution1.png" alt="Abstract Factory - Solution 1"/>
  <p>Product families and their variants.</p>
</div>

You need a way to create individual furniture objects so that they match other objects of the same family. Customers get quite mad when they receive non-matching furniture.

<div align="center">
  <img src="./assets/abstract-factory - solution2.png" alt="Abstract Factory - Solution 2"/>
  <p>A Modern-style sofa doesn’t match Victorian-style chairs.</p>
</div>

Also, you don’t want to change existing code when adding new products or families of products to the program. Furniture vendors update their catalogs very often, and you wouldn’t want to change the core code each time it happens.

### Solution

The first thing the Abstract Factory pattern suggests is to explicitly declare interfaces for each distinct product of the product family (e.g., chair, sofa or coffee table). Then you can make all variants of products follow those interfaces. For example, all chair variants can implement the **Chair** interface; all coffee table variants can implement the **CoffeeTable** interface, and so on.

<div align="center">
  <img src="./assets/abstract-factory - solution3.png" alt="Abstract Factory - Solution 3"/>
  <p>All variants of the same object must be moved to a single class hierarchy.</p>
</div>

The next move is to declare the _Abstract Factory_—an interface with a list of creation methods for all products that are part of the product family (for example, **createChair**, **createSofa** and **createCoffeeTable**). These methods must return **abstract** product types represented by the interfaces we extracted previously: **Chair**, **Sofa**, **CoffeeTable** and so on.

<div align="center">
  <img src="./assets/abstract-factory - solution4.png" alt="Abstract Factory - Solution 4"/>
  <p>Each concrete factory corresponds to a specific product variant.</p>
</div>

Now, how about the product variants? For each variant of a product family, we create a separate factory class based on the **AbstractFactory** interface. A factory is a class that returns products of a particular kind. For example, the **ModernFurnitureFactory** can only create **ModernChair**, **ModernSofa** and **ModernCoffeeTable** objects.

The client code has to work with both factories and products via their respective abstract interfaces. This lets you change the type of a factory that you pass to the client code, as well as the product variant that the client code receives, without breaking the actual client code.

<div align="center">
  <img src="./assets/abstract-factory - solution5.png" alt="Abstract Factory - Solution 5"/>
  <p>The client shouldn’t care about the concrete class of the factory it works with.</p>
</div>

Say the client wants a factory to produce a chair. The client doesn’t have to be aware of the factory’s class, nor does it matter what kind of chair it gets. Whether it’s a Modern model or a Victorian-style chair, the client must treat all chairs in the same manner, using the abstract **Chair** interface. With this approach, the only thing that the client knows about the chair is that it implements the **sitOn** method in some way. Also, whichever variant of the chair is returned, it’ll always match the type of sofa or coffee table produced by the same factory object.

There’s one more thing left to clarify: if the client is only exposed to the abstract interfaces, what creates the actual factory objects? Usually, the application creates a concrete factory object at the initialization stage. Just before that, the app must select the factory type depending on the configuration or the environment settings.

### Structure

<div align="center">
  <img src="./assets/abstract-factory - solution6.png" alt="Abstract Factory - Solution 6"/>
</div>

### Pros

1. You can be sure that the products you’re getting from a factory are compatible with each other.
2. You avoid tight coupling between concrete products and client code.
3. _Single Responsibility Principle_. You can extract the product creation code into one place, making the code easier to support.
4. _Open/Closed Principle_. You can introduce new variants of products without breaking existing client code.

### Cons

1. The code may become more complicated than it should be, since a lot of new interfaces and classes are introduced along with the pattern.

## Builder

### Intent

The Builder pattern is a creational design pattern that separates the construction of a complex object from its representation, allowing the same construction process to create different representations. It involves a director class that orchestrates the construction through a builder interface, ensuring a step-by-step construction of the object.

### Problem

Imagine a complex object that requires laborious, step-by-step initialization of many fields and nested objects. Such initialization code is usually buried inside a monstrous constructor with lots of parameters. Or even worse: scattered all over the client code.

<div align="center">
  <img src="./assets/builder - solution1.png" alt="Builder - Solution 1"/>
  <p>
    You might make the program too complex by creating a subclass for every possible configuration of an object.
  </p>
</div>

For example, let’s think about how to create a **House** object. To build a simple house, you need to construct four walls and a floor, install a door, fit a pair of windows, and build a roof. But what if you want a bigger, brighter house, with a backyard and other goodies (like a heating system, plumbing, and electrical wiring)?

The simplest solution is to extend the base **House** class and create a set of subclasses to cover all combinations of the parameters. But eventually you’ll end up with a considerable number of subclasses. Any new parameter, such as the porch style, will require growing this hierarchy even more.

There’s another approach that doesn’t involve breeding subclasses. You can create a giant constructor right in the base **House** class with all possible parameters that control the house object. While this approach indeed eliminates the need for subclasses, it creates another problem.

<div align="center">
  <img src="./assets/builder - solution2.png" alt="Builder - Solution 2"/>
  <p>
    The constructor with lots of parameters has its downside: not all the parameters are needed at all times.
  </p>
</div>

In most cases most of the parameters will be unused, making **the constructor calls pretty ugly**. For instance, only a fraction of houses have swimming pools, so the parameters related to swimming pools will be useless nine times out of ten.

### Solution

The Builder pattern suggests that you extract the object construction code out of its own class and move it to separate objects called _builders_.

<div align="center">
  <img src="./assets/builder - solution3.png" alt="Builder - Solution 3"/>
  <p>
    The Builder pattern lets you construct complex objects step by step. The Builder doesn’t allow other objects to access the product while it’s being built.
  </p>
</div>

The pattern organizes object construction into a set of steps (**buildWalls**, **buildDoor**, etc.). To create an object, you execute a series of these steps on a builder object. The important part is that you don’t need to call all of the steps. You can call only those steps that are necessary for producing a particular configuration of an object.

Some of the construction steps might require different implementation when you need to build various representations of the product. For example, walls of a cabin may be built of wood, but the castle walls must be built with stone.

In this case, you can create several different builder classes that implement the same set of building steps, but in a different manner. Then you can use these builders in the construction process (i.e., an ordered set of calls to the building steps) to produce different kinds of objects.

<div align="center">
  <img src="./assets/builder - solution4.png" alt="Builder - Solution 4"/>
  <p>Different builders execute the same task in various ways.</p>
</div>

For example, imagine a builder that builds everything from wood and glass, a second one that builds everything with stone and iron and a third one that uses gold and diamonds. By calling the same set of steps, you get a regular house from the first builder, a small castle from the second and a palace from the third. However, this would only work if the client code that calls the building steps is able to interact with builders using a common interface.

**Director**

You can go further and extract a series of calls to the builder steps you use to construct a product into a separate class called director. The director class defines the order in which to execute the building steps, while the builder provides the implementation for those steps.

<div align="center">
  <img src="./assets/builder - solution5.png" alt="Builder - Solution 5"/>
  <p>The director knows which building steps to execute to get a working product.</p>
</div>

Having a director class in your program isn’t strictly necessary. You can always call the building steps in a specific order directly from the client code. However, the director class might be a good place to put various construction routines so you can reuse them across your program.

In addition, the director class completely hides the details of product construction from the client code. The client only needs to associate a builder with a director, launch the construction with the director, and get the result from the builder.

### Structure

<div align="center">
  <img src="./assets/builder - solution6.png" alt="Builder - Solution 6"/>
</div>

### Pros

1. You can construct objects step-by-step, defer construction steps or run steps recursively.
2. You can reuse the same construction code when building various representations of products.
3. _Single Responsibility Principle_. You can isolate complex construction code from the business logic of the product.

### Cons

1. The overall complexity of the code increases since the pattern requires creating multiple new classes.

## Prototype

### Intent

The Prototype Pattern is a design pattern used in software engineering to create new objects by copying an existing object, known as the prototype. Rather than creating instances through conventional instantiation, the prototype serves as a blueprint from which new objects are cloned.

### Problem

Say you have an object, and you want to create an exact copy of it. How would you do it? First, you have to create a new object of the same class. Then you have to go through all the fields of the original object and copy their values over to the new object.

Nice! But there’s a catch. Not all objects can be copied that way because some of the object’s fields may be private and not visible from outside of the object itself.

<div align="center">
  <img src="./assets/prototype - solution1.png" alt="Builder - Solution 1"/>
  <p>Copying an object “from the outside” isn’t always possible.</p>
</div>

There’s one more problem with the direct approach. Since you have to know the object’s class to create a duplicate, your code becomes dependent on that class. If the extra dependency doesn’t scare you, there’s another catch. Sometimes you only know the interface that the object follows, but not its concrete class, when, for example, a parameter in a method accepts any objects that follow some interface.

### Solution

The Prototype pattern delegates the cloning process to the actual objects that are being cloned. The pattern declares a common interface for all objects that support cloning. This interface lets you clone an object without coupling your code to the class of that object. Usually, such an interface contains just a single **clone** method.

The implementation of the **clone** method is very similar in all classes. The method creates an object of the current class and carries over all of the field values of the old object into the new one. You can even copy private fields because most programming languages let objects access private fields of other objects that belong to the same class.

An object that supports cloning is called a _prototype_. When your objects have dozens of fields and hundreds of possible configurations, cloning them might serve as an alternative to subclassing.

<div align="center">
  <img src="./assets/prototype - solution2.png" alt="Builder - Solution 2"/>
  <p>Pre-built prototypes can be an alternative to subclassing.</p>
</div>

Here’s how it works: you create a set of objects, configured in various ways. When you need an object like the one you’ve configured, you just clone a prototype instead of constructing a new object from scratch.

### Structure

<div align="center">
  <img src="./assets/prototype - solution3.png" alt="Builder - Solution 3"/>
</div>

<div align="center">
  <img src="./assets/prototype - solution4.png" alt="Builder - Solution 4"/>
</div>

### Pros

1. You can clone objects without coupling to their concrete classes.
2. You can get rid of repeated initialization code in favor of cloning pre-built prototypes.
3. You can produce complex objects more conveniently.
4. You get an alternative to inheritance when dealing with configuration presets for complex objects.

### Cons

1. Cloning complex objects that have circular references might be very tricky.

## Singleton

Singleton is a creational design pattern that lets you ensure that a class has only one instance, while providing a global access point to this instance.

### Problem

The Singleton pattern solves two problems at the same time, violating the _Single Responsibility Principle_:

1. **Ensure that a class has just a single instance**. Why would anyone want to control how many instances a class has? The most common reason for this is to control access to some shared resource—for example, a database or a file.

Here’s how it works: imagine that you created an object, but after a while decided to create a new one. Instead of receiving a fresh object, you’ll get the one you already created.

Note that this behavior is impossible to implement with a regular constructor since a constructor call **must** always return a new object by design.

<div align="center">
  <img src="./assets/singleton - solution1.png" alt="Builder - Solution 1"/>
  <p>Clients may not even realize that they’re working with the same object all the time.</p>
</div>

2. **Provide a global access point to that instance**. Remember those global variables that you (all right, me) used to store some essential objects? While they’re very handy, they’re also very unsafe since any code can potentially overwrite the contents of those variables and crash the app.

Just like a global variable, the Singleton pattern lets you access some object from anywhere in the program. However, it also protects that instance from being overwritten by other code.

There’s another side to this problem: you don’t want the code that solves problem #1 to be scattered all over your program. It’s much better to have it within one class, especially if the rest of your code already depends on it.

Nowadays, the Singleton pattern has become so popular that people may call something a _singleton_ even if it solves just one of the listed problems.

### Solution

All implementations of the Singleton have these two steps in common:

- Make the default constructor private, to prevent other objects from using the **new** operator with the Singleton class.
- Create a static creation method that acts as a constructor. Under the hood, this method calls the private constructor to create an object and saves it in a static field. All following calls to this method return the cached object.

If your code has access to the Singleton class, then it’s able to call the Singleton’s static method. So whenever that method is called, the same object is always returned.

### Structure

<div align="center">
  <img src="./assets/singleton - solution2.png" alt="Builder - Solution 2"/>
</div>

### Pros

1. You can be sure that a class has only a single instance.
2. You gain a global access point to that instance.
3. The singleton object is initialized only when it’s requested for the first time.

### Cons

1. Violates the _Single Responsibility Principle_. The pattern solves two problems at the time.
2. The Singleton pattern can mask bad design, for instance, when the components of the program know too much about each other.
3. The pattern requires special treatment in a multithreaded environment so that multiple threads won’t create a singleton object several times.
4. It may be difficult to unit test the client code of the Singleton because many test frameworks rely on inheritance when producing mock objects. Since the constructor of the singleton class is private and overriding static methods is impossible in most languages, you will need to think of a creative way to mock the singleton. Or just don’t write the tests. Or don’t use the Singleton pattern.

## Adapter

### Intent

**The Adapter Pattern **facilitates the interaction between two incompatible interfaces. It allows classes with incompatible interfaces to work together by creating a bridge between them.

In the Adapter Pattern, an adapter class converts the interface of one class into another interface that a client expects. This allows classes to work together that couldn't otherwise because of incompatible interfaces. The adapter pattern can be implemented either using inheritance (class adapter) or composition (object adapter).

The main components of the Adapter Pattern are:

- Target: This is the interface that the client code expects to interact with.

- Adapter: This is the class that adapts the interface of the Adaptee to the Target interface. It implements the Target interface and holds a reference to an instance of the Adaptee.

- Adaptee: This is the class that has the interface that needs to be adapted to the Target interface. It's the class that the client code cannot directly interact with.

The Adapter Pattern is particularly useful when integrating legacy code or third-party libraries into a new system, as it allows you to work with existing code without modifying it directly.

### Problem

Imagine that you’re creating a stock market monitoring app. The app downloads the stock data from multiple sources in XML format and then displays nice-looking charts and diagrams for the user.

At some point, you decide to improve the app by integrating a smart 3rd-party analytics library. But there’s a catch: the analytics library only works with data in JSON format.

<div align="center">
  <img src="./assets/adapter - solution1.png" alt="Adapter - Solution 1"/>
  <p>
    You can’t use the analytics library “as is” because it expects the data in a format that’s incompatible with your app.
  </p>
</div>

You could change the library to work with XML. However, this might break some existing code that relies on the library. And worse, you might not have access to the library’s source code in the first place, making this approach impossible.

### Solution

You can create an _adapter_. This is a special object that converts the interface of one object so that another object can understand it.

An adapter wraps one of the objects to hide the complexity of conversion happening behind the scenes. The wrapped object isn’t even aware of the adapter. For example, you can wrap an object that operates in meters and kilometers with an adapter that converts all of the data to imperial units such as feet and miles.

Adapters can not only convert data into various formats but can also help objects with different interfaces collaborate. Here’s how it works:

- The adapter gets an interface, compatible with one of the existing objects.
- Using this interface, the existing object can safely call the adapter’s methods.
- Upon receiving a call, the adapter passes the request to the second object, but in a format and order that the second object expects.

Sometimes it’s even possible to create a two-way adapter that can convert the calls in both directions.

<div align="center">
  <img src="./assets/adapter - solution2.png" alt="Adapter - Solution 2"/>
</div>

Let’s get back to our stock market app. To solve the dilemma of incompatible formats, you can create XML-to-JSON adapters for every class of the analytics library that your code works with directly. Then you adjust your code to communicate with the library only via these adapters. When an adapter receives a call, it translates the incoming XML data into a JSON structure and passes the call to the appropriate methods of a wrapped analytics object.

### Structure

**Object adapter**

This implementation uses the object composition principle: the adapter implements the interface of one object and wraps the other one. It can be implemented in all popular programming languages.

<div align="center">
  <img src="./assets/adapter - solution3.png" alt="Adapter - Solution 3"/>
</div>

**Class adapter**

This implementation uses inheritance: the adapter inherits interfaces from both objects at the same time. Note that this approach can only be implemented in programming languages that support multiple inheritance, such as C++.

<div align="center">
  <img src="./assets/adapter - solution4.png" alt="Adapter - Solution 4"/>
</div>

### Props

1. _Single Responsibility Principle_. You can separate the interface or data conversion code from the primary business logic of the program.
2. _Open/Closed Principle_. You can introduce new types of adapters into the program without breaking the existing client code, as long as they work with the adapters through the client interface.
3. The overall complexity of the code increases because you need to introduce a set of new interfaces and classes. Sometimes it’s simpler just to change the service class so that it matches the rest of your code.

### Cons

1. The overall complexity of the code increases because you need to introduce a set of new interfaces and classes. Sometimes it’s simpler just to change the service class so that it matches the rest of your code.

## Decorator

**Decorator** is a structural design pattern that lets you attach new behaviors to objects by placing these objects inside special wrapper objects that contain the behaviors.

Here's how the Decorator Pattern works:

- Component: This is the base interface or abstract class that defines the common interface for all concrete components and decorators. It represents the object that will be decorated.

- Concrete Component: This is the basic implementation of the Component interface. It defines the core behavior of the object to which additional responsibilities can be added.

- Decorator: This is the abstract class that implements the Component interface and maintains a reference to a Component object. It acts as a wrapper for the concrete component, adding additional behavior dynamically.

- Concrete Decorator: These are the classes that extend the Decorator class and provide specific additional functionality. They override methods of the Component interface to add their own behavior before or after delegating to the wrapped Component object.

The Decorator Pattern allows you to stack decorators on top of each other, adding multiple layers of functionality to an object. It promotes a flexible design where responsibilities can be added or removed independently, and where the client code can interact with objects without being aware of the presence of decorators.

## Problem

Imagine that you’re working on a notification library which lets other programs notify their users about important events.

The initial version of the library was based on the _Notifier_ class that had only a few fields, a constructor and a single _send_ method. The method could accept a message argument from a client and send the message to a list of emails that were passed to the notifier via its constructor. A third-party app which acted as a client was supposed to create and configure the notifier object once, and then use it each time something important happened.

<div align="center">
  <img src="./assets/decorator - solution1.png" alt="Decorator - Solution 1"/>
  <p> A program could use the notifier class to send notifications about important events to a predefined set of emails.</p>
</div>

At some point, you realize that users of the library expect more than just email notifications. Many of them would like to receive an SMS about critical issues. Others would like to be notified on Facebook and, of course, the corporate users would love to get Slack notifications.

<div align="center">
  <img src="./assets/decorator - solution2.png" alt="Decorator - Solution 2"/>
  <p> Each notification type is implemented as a notifier’s subclass. </p>
</div>

How hard can that be? You extended the _Notifier_ class and put the additional notification methods into new subclasses. Now the client was supposed to instantiate the desired notification class and use it for all further notifications.

But then someone reasonably asked you, “Why can’t you use several notification types at once? If your house is on fire, you’d probably want to be informed through every channel.”

You tried to address that problem by creating special subclasses which combined several notification methods within one class. However, it quickly became apparent that this approach would bloat the code immensely, not only the library code but the client code as well.

<div align="center">
  <img src="./assets/decorator - solution3.png" alt="Decorator - Solution 3"/>
  <p> Combinatorial explosion of subclasses. </p>
</div>

You have to find some other way to structure notifications classes so that their number won’t accidentally break some Guinness record.

## Solution

Extending a class is the first thing that comes to mind when you need to alter an object’s behavior. However, inheritance has several serious caveats that you need to be aware of.

- Inheritance is static. You can’t alter the behavior of an existing object at runtime. You can only replace the whole object with another one that’s created from a different subclass.

- Subclasses can have just one parent class. In most languages, inheritance doesn’t let a class inherit behaviors of multiple classes at the same time.

One of the ways to overcome these caveats is by using _Aggregation_ or _Composition_

- Aggregation: object A contains objects B; B can live without A.
- Composition: object A consists of objects B; A manages life cycle of B; B can’t live without A.

instead of _Inheritance_. Both of the alternatives work almost the same way: one object _has_ a reference to another and delegates it some work, whereas with inheritance, the object itself _is_ able to do that work, inheriting the behavior from its superclass.

With this new approach you can easily substitute the linked “helper” object with another, changing the behavior of the container at runtime. An object can use the behavior of various classes, having references to multiple objects and delegating them all kinds of work. Aggregation/composition is the key principle behind many design patterns, including Decorator. On that note, let’s return to the pattern discussion.

<div align="center">
  <img src="./assets/decorator - solution4.png" alt="Decorator - Solution 4"/>
  <p> Inheritance vs. Aggregation </p>
</div>

“Wrapper” is the alternative nickname for the Decorator pattern that clearly expresses the main idea of the pattern. A _wrapper_ is an object that can be linked with some _target object_. The wrapper contains the same set of methods as the target and delegates to it all requests it receives. However, the wrapper may alter the result by doing something either before or after it passes the request to the target.

When does a simple wrapper become the real decorator? As I mentioned, the wrapper implements the same interface as the wrapped object. That’s why from the client’s perspective these objects are identical. Make the wrapper’s reference field accept any object that follows that interface. This will let you cover an object in multiple wrappers, adding the combined behavior of all the wrappers to it.

In our notifications example, let’s leave the simple email notification behavior inside the base _Notifier_ class, but turn all other notification methods into decorators.

<div align="center">
  <img src="./assets/decorator - solution5.png" alt="Decorator - Solution 5"/>
  <p> Various notification methods become decorators. </p>
</div>

The client code would need to wrap a basic notifier object into a set of decorators that match the client’s preferences. The resulting objects will be structured as a stack.

<div align="center">
  <img src="./assets/decorator - solution6.png" alt="Decorator - Solution 6"/>
  <p> Apps might configure complex stacks of notification decorators. </p>
</div>

The last decorator in the stack would be the object that the client actually works with. Since all decorators implement the same interface as the base notifier, the rest of the client code won’t care whether it works with the “pure” notifier object or the decorated one.

We could apply the same approach to other behaviors such as formatting messages or composing the recipient list. The client can decorate the object with any custom decorators, as long as they follow the same interface as the others.

### Structure

<div align="center">
  <img src="./assets/decorator - solution7.png" alt="Decorator - Solution 7"/>
</div>

### Pros

1. You can extend an object’s behavior without making a new subclass.
2. You can add or remove responsibilities from an object at runtime.
3. You can combine several behaviors by wrapping an object into multiple decorators.
4. _Single Responsibility Principle_. You can divide a monolithic class that implements many possible variants of behavior into several smaller classes.

### Cons

1. It’s hard to remove a specific wrapper from the wrappers stack.
2. It’s hard to implement a decorator in such a way that its behavior doesn’t depend on the order in the decorators stack.
3. The initial configuration code of layers might look pretty ugly.

## Facade

### Intent

The Facade Pattern is a design pattern used in software engineering to provide a simplified interface to a complex system of classes, libraries, or frameworks. It hides the complexities of the underlying system and provides a unified interface that is easier to use.

Here's how the Facade Pattern works:

- Facade: This is a class that provides a simple interface to the complex subsystem. It encapsulates the interactions with the various components of the subsystem and presents a higher-level interface that clients can use to access the subsystem functionalities.

- Subsystem Classes: These are the classes that represent the various components of the complex system. They contain the actual implementation of the system's functionalities.

The Facade Pattern allows clients to interact with the system through a single interface provided by the Facade class, without needing to understand the complexities of the subsystem. This simplifies client code, reduces dependencies on the subsystem classes, and promotes loose coupling between clients and the subsystem.

### Problem

Imagine that you must make your code work with a broad set of objects that belong to a sophisticated library or framework. Ordinarily, you’d need to initialize all of those objects, keep track of dependencies, execute methods in the correct order, and so on.

As a result, the business logic of your classes would become tightly coupled to the implementation details of 3rd-party classes, making it hard to comprehend and maintain.

### Solution

A facade is a class that provides a simple interface to a complex subsystem which contains lots of moving parts. A facade might provide limited functionality in comparison to working with the subsystem directly. However, it includes only those features that clients really care about.

Having a facade is handy when you need to integrate your app with a sophisticated library that has dozens of features, but you just need a tiny bit of its functionality.

For instance, an app that uploads short funny videos with cats to social media could potentially use a professional video conversion library. However, all that it really needs is a class with the single method **encode(filename, format)**. After creating such a class and connecting it with the video conversion library, you’ll have your first facade.

### Structure

<div align="center">
  <img src="./assets/facade - solution1.png" alt="Facade - Solution 1"/>
</div>

### Pros

1. You can isolate your code from the complexity of a subsystem.

### Cons

2. A facade can become **a god object** coupled to all classes of an app.

## Observer Pattern

### Intent

The Observer Pattern is a behavioral design pattern that defines a one-to-many dependency between objects. When the state of one object changes, all its dependents (observers) are notified and updated automatically. This pattern is also known as the publish-subscribe pattern.

Here's how the Observer Pattern works:

- Subject (Observable): This is the object that is being observed. It maintains a list of observers and provides methods to attach, detach, and notify observers of changes in its state.

- Observer: This is the interface or abstract class that defines the method(s) to be called when the state of the subject changes. Observers register with the subject to receive updates.

- Concrete Subject: This is the concrete implementation of the subject interface. It maintains the state that observers are interested in and notifies observers when the state changes.

- Concrete Observer: This is the concrete implementation of the observer interface. It implements the method(s) defined in the observer interface to update its state when notified by the subject.

The Observer Pattern allows for loose coupling between the subject and its observers. Subjects don't need to know the specific classes of their observers, only that they implement the observer interface. This promotes flexibility and reusability in the design of the system.

### Problem

Imagine that you have two types of objects: a **Customer** and a **Store**. The customer is very interested in a particular brand of product (say, it’s a new model of the iPhone) which should become available in the store very soon.

The customer could visit the store every day and check product availability. But while the product is still en route, most of these trips would be pointless.

<div align="center">
  <img src="./assets/observer - solution1.png" alt="Observer - Solution 1"/>
  <p>Visiting the store vs. sending spam</p>
</div>

On the other hand, the store could send tons of emails (which might be considered spam) to all customers each time a new product becomes available. This would save some customers from endless trips to the store. At the same time, it’d upset other customers who aren’t interested in new products.

It looks like we’ve got a conflict. Either the customer wastes time checking product availability or the store wastes resources notifying the wrong customers.

### Solution

The object that has some interesting state is often called _subject_, but since it’s also going to notify other objects about the changes to its state, we’ll call it _publisher_. All other objects that want to track changes to the publisher’s state are called _subscribers_.

The Observer pattern suggests that you add a subscription mechanism to the publisher class so individual objects can subscribe to or unsubscribe from a stream of events coming from that publisher. Fear not! Everything isn’t as complicated as it sounds. In reality, this mechanism consists of 1) an array field for storing a list of references to subscriber objects and 2) several public methods which allow adding subscribers to and removing them from that list.

<div align="center">
  <img src="./assets/observer - solution2.png" alt="Observer - Solution 2"/>
  <p>A subscription mechanism lets individual objects subscribe to event notifications.</p>
</div>

Now, whenever an important event happens to the publisher, it goes over its subscribers and calls the specific notification method on their objects.

Real apps might have dozens of different subscriber classes that are interested in tracking events of the same publisher class. You wouldn’t want to couple the publisher to all of those classes. Besides, you might not even know about some of them beforehand if your publisher class is supposed to be used by other people.

That’s why it’s crucial that all subscribers implement the same interface and that the publisher communicates with them only via that interface. This interface should declare the notification method along with a set of parameters that the publisher can use to pass some contextual data along with the notification.

<div align="center">
  <img src="./assets/observer - solution3.png" alt="Observer - Solution 3"/>
  <p>Publisher notifies subscribers by calling the specific notification method on their objects.</p>
</div>

If your app has several different types of publishers and you want to make your subscribers compatible with all of them, you can go even further and make all publishers follow the same interface. This interface would only need to describe a few subscription methods. The interface would allow subscribers to observe publishers’ states without coupling to their concrete classes.

### Structure

<div align="center">
  <img src="./assets/observer - solution4.png" alt="Observer - Solution 4"/>
</div>

### Pros

1. _Open/Closed Principle_. You can introduce new subscriber classes without having to change the publisher’s code (and vice versa if there’s a publisher interface).

2. You can establish relations between objects at runtime.

### Cons

1. Subscribers are notified in random order.
