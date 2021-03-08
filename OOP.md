# OOP
**Object-oriented programming** is [Programming Paradigm](https://en.wikipedia.org/wiki/Programming_paradigm) based on the concept of wrapping pieces of data, and behavior related to that
data, into special bundles called *objects*.

##### Objects and Classes
- **class**: the definitions for the data format and available procedures for a given type or class of object.
- **Object**: Instance of Classes

Procedures in Programming world Known as *methods* ad a data;variables are also know as properties or attributes. this leads to the following Terms:
- **Class variable**: Belong to the *class*, and there is one copy of that at all
- **Instance variable** or attribute: data that belong to *individual object*, each object has its own copy of that
- **Member variable**: Belong to the both class and objects, defined by a particular class.
- **Class Method**: Belong to the *Class* as a whole and have access to Class variables only and inputs from procedures call. 
- **Instance Method**: Belong to *individual objects* and have access to instance variables of specific objects they are called on.

## Pillars Of OOP
Object Oriented Programming is based on 4 pillars that differentiate it from the other paradigms.

- Abstraction
- Polymorphism (*we use it in different way in python*)
- Inheritance
- Encapsulation

### Abstraction
In Abstraction we Won't represent any specific Details, we just define them and will pass it to childes to define it in detail. 

### Encapsulation
To start Your car engine, you need only to turn a key or press a button. You don't need to connect wires under the hood, rotate the crankshaft and cylinders, and initiate the power cycle of the engine.These details are hidden under hood and we use a interface for convenient or sort of security.

![image](https://cdn1.bbcode0.com/uploads/2021/3/7/6d2b1e13a0d7e8ed16abfdc6202e1eb9-full.jpg)

### Inheritance
Inheritance is the ability to build new classes on top of existing ones. The main benefit of inheritance is code reuse. If you want to create a class that's slightly different from an existing one, there's no need to duplicate code. Instead, you extend the existing class and put the extra functionality into a resulting subclass, which inherits fields and methods of the superclass.

### Polymorphism

This is kind of like Abstraction But with this difference that we won't define anything abstract, actually in Polymorphism we define methods that has different behaviors base on the inputs.

## Relations Between Objects

![image](https://upload.wikimedia.org/wikipedia/commons/thumb/9/93/Uml_classes_en.svg/300px-Uml_classes_en.svg.png)

- **Association**: Association is a type of relationship in which one object uses or interacts with another.
- **Dependency**: Dependency is a weaker variant of association that usually implies that there's no permanent link between objects. Dependency typically (but not always) implies that an object accepts another object as a method parameter, instantiates, or uses another object.
- **Composition**: Composition is a “whole-part” relationship between two objects, one of which is composed of one or more instances of the other. The distinction between this relation and others is that the component can only exist as a *part of* the container.
- **Aggregation**: Aggregation is a less strict variant of composition, where one object merely contains a reference to another. The container doesn't control the life cycle of the component. The component can exist without the container and can be linked to several containers at the same time.

# Design Patterns

**Design patterns** are typical solutions to commonly occurring
problems in software design.

You can't just find a pattern and copy it into your program. The pattern is not a specific piece of code, but a general concept for solving a particular problem. You can follow the pattern details and implement a solution that suits the realities of your own program.

algorithm and Design Patterns are not same, algorithm always defines a clear set of actions that can achieve some goal, a pattern is a more high-level description of a solution.

> Features of Good Design:
> - Code Reuse: Code reuse is one of the most common ways to reduce development costs. The intent is pretty obvious: instead of developing something over and over from scratch, why don't we reuse existing code in new projects?
> - Extensibility: Change is the only constant thing in a programmer's life.
> So we must be prepared for that.
There is 3 Groups of Patterns:

- **Creational Patterns** provide object creation mechanisms that increase flexibility and reuse of existing code.
- **Structural patterns** explain how to assemble objects and classes into larger structures, while keeping the structures flexible and efficient.
- **Behavioral patterns** take care of effective communication and the assignment of responsibilities between objects.

we will Continue each one of this Categories individually. 




# Resources

- [Object Orient definition in Wiki](https://en.wikipedia.org/wiki/Object-oriented_programming)
- [Refactoring](https://refactoring.guru/refactoring)
- [Class Diagram Definition in Wiki](https://en.wikipedia.org/wiki/Class_diagram)