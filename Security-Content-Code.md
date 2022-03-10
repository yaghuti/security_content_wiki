The code in security content is using the following concepts:
- Clean Architecture
- Software Engineering Design Patterns
- Test Driven Development

Concepts from the following books about clean architecture are followed to structure the code:
- [Clean Architecture: A Craftsman’s Guide to Software Structure and Design](https://www.oreilly.com/library/view/clean-architecture-a/9780134494272/)
- [Implementing the Clean Architecture](https://leanpub.com/implementing-the-clean-architecture)

# Clean Architecture
![](https://blog.cleancoder.com/uncle-bob/images/2012-08-13-the-clean-architecture/CleanArchitecture.jpg)

The Clean Architecture provides the following advantages:
- Independent of Frameworks
- Testability
- Independent of Database
- Independent of any external agency

An important concept of Clean Architecture is the dependency rule. This rule says that source code dependencies can only point inwards. Nothing in an inner circle can know anything at all about something in an outer circle. In particular, the name of something declared in an outer circle must not be mentioned by the code in the an inner circle. That includes, functions, classes. variables, or any other named software entity. This leads to that changes in code of an outer circle has no impact on the code of an inner circle.

[More information](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)

Based on this guidelines, the security content project is structured as following under bin/contentctl_project:
* contentctl_core: Contains the two inner circles consisting of Domain and Application. Domain consists of Entities which are objects like detections, stories and so on. The application layer consists of Interface Adapter, Interface Builder, Factories and Use Cases.
* contentctl_infrastructure: Contains the third inner circle and contains the Implementation of Adapter, Builder and Database specific writer and reader classes.



# Software Engineering Design Patterns
The following software design patterns are used:
- [Builder](https://sourcemaking.com/design_patterns/builder)
- [Factory](https://sourcemaking.com/design_patterns/factory_method)
- [Adapter](https://sourcemaking.com/design_patterns/adapter)

## Builder

### Problem
An application needs to create the elements of a complex aggregate. The specification for the aggregate exists on secondary storage and one of many representations needs to be built in primary storage.

### Intent
- Separate the construction of a complex object from its representation so that the same construction process can create different representations.
- Parse a complex representation, create one of several targets.

[More information](https://sourcemaking.com/design_patterns/builder)

## Factory

### Problem
A framework needs to standardize the architectural model for a range of applications, but allow for individual applications to define their own domain objects and provide for their instantiation.

### Intent
- Define an interface for creating an object, but let subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses.
- Defining a "virtual" constructor.
- The new operator considered harmful.

[More information](https://sourcemaking.com/design_patterns/factory_method)

## Adapter

### Problem
An "off the shelf" component offers compelling functionality that you would like to reuse, but its "view of the world" is not compatible with the philosophy and architecture of the system currently being developed.

### Intent
- Convert the interface of a class into another interface clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.
- Wrap an existing class with a new interface.
- Impedance match an old component to a new system

[More information](https://sourcemaking.com/design_patterns/adapter)

# Test Driven Development
Test Driven Development (TDD) is software development approach in which test cases are developed to specify and validate what the code will do. In simple terms, test cases for each functionality are created and tested first and if the test fails then the new code is written in order to pass the test and making code simple and bug-free. Test-Driven Development starts with designing and developing tests for every small functionality of an application. 

For test driven development in security content, pytest is used. Pytest will test every python file which starts with test_*. Inside the python file, it will run every method which starts with test_*. Every class in security content should have a corresponding test file. Pytest can be use in the following way:
````
export PYTHONPATH="/path/to/security_content/"
pytest -s bin/contentctl_project 
````

[More information](https://www.guru99.com/test-driven-development.html)