The code in security content is using the following concepts to improve quality, maintainability and reduce complexity:
- Domain Driven Design
- Software Engineering Design Patterns
- Test Driven Development

Concepts from the following books about clean architecture are followed to structure the code:
- [Clean Architecture: A Craftsman’s Guide to Software Structure and Design](https://www.oreilly.com/library/view/clean-architecture-a/9780134494272/)
- [Implementing the Clean Architecture](https://leanpub.com/implementing-the-clean-architecture)

# Domain Driven Design


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

## Factory

### Problem
A framework needs to standardize the architectural model for a range of applications, but allow for individual applications to define their own domain objects and provide for their instantiation.

### Intent
- Define an interface for creating an object, but let subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses.
- Defining a "virtual" constructor.
- The new operator considered harmful.

## Adapter

### Problem
An "off the shelf" component offers compelling functionality that you would like to reuse, but its "view of the world" is not compatible with the philosophy and architecture of the system currently being developed.

### Intent
- Convert the interface of a class into another interface clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.
- Wrap an existing class with a new interface.
- Impedance match an old component to a new system

# Test Driven Development