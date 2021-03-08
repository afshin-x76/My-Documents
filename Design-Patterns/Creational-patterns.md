# Creational Patterns

Creational patterns provide various object creation mechanisms, which increase flexibility and reuse of existing code. Finally behavioral patterns are about identifying common communication patterns between objects and realize these patterns.

Below is a list of Design Patterns
- **Abstract Factory**: lets you produce families of related objects without specifying their concrete classes.	
- **Factory Method**: provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.
- **Builder**:	lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code.
- **Prototype**: lets you copy existing objects without making your code dependent on their classes.
- **Singleton**: lets you ensure that a class has only one instance, while providing a global access point to this instance.


## Prototype
### Problem
The Problem is that when we want to use an object and make a so many copy of that. maybe your solution is that to copy all the fields of the parent class and make your change on them and create your own class. we can count the problem of this way in below:

1.Not all objects can be copied that way because some of the object's fields may be private and not visible from outside of the object itself.
2.Since you have to know the object's class to create a duplicate, your code becomes dependent on that class.
3.Sometimes you only know the interface that the object follows, but not its concrete class, when, for example, a parameter in a method accepts any objects that follow some interface.

### Solution
The Prototype pattern delegates the cloning process to the actual objects that are being cloned. The pattern declares a common interface for all objects that support cloning. This interface lets you clone an object without coupling your code to the class of that object. Usually, such an interface contains just a single clone method.

Clone method in Interface class will create an object from main class and caries all the field and method and functionality of that object to new class.

An object that support clone method is a prototype

### Structure 

#### Basic implementation

![image](https://refactoring.guru/images/patterns/diagrams/prototype/structure.png)

#### Prototype registry implementation

![image](https://refactoring.guru/images/patterns/diagrams/prototype/structure-prototype-cache.png)

## Applicability

- You can use Prototype when you don't want make your code dependent to concrete class
- when your code will work with the object passes from the 3rd-party code via interface, you can use prototype.
- when you need objects with different initialize, you don't need to use subclass for each one, this is the best idea to use prototype.

## How to Implement

1. Create an prototype interface with clone method



