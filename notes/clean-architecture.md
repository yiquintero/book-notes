# Clean Architecture
Notes from the book *Clean Architecture - A Craftman's Guide to Software Structure and Design* by Robert C. Martin

:toc:

## Part I: Introduction
## Chapter 1: What is Design and Architecture

There is **no** difference between architecture and design, despite preconceptions that architecture operates at a high level and design takes care of low-level details. Little, low-level details support all the high-level decisions.

The goal of software architecture is to minimize the human resources required to build and mantain the required system.

The bigger lie that developers buy into is the notion that writing messy code makes them go fast in the short term, and just slows them down in the long term. The fact is that making messes is always slower than staying clean, no matter which time scale you are using. *The only way to go fast is to go well.*

## Chapter 2: A Tale of Two Values

Every software system provides two different values to the stakeholders: behaviour and structure.
  * **Behaviour**: the software behaves in a way that aligns with stakeholders' goals
  * **Structure**: the software must be easy to change. The diffuclty in making a change should be proportional only to the scope of the change, and not to the *shape* of the change. The more the architecture prefers one shape over another, the more likely new features will be harder and harder to fit into the structure.

Which is more important?
* If you give me a program that works perfectly but is impossible to change, tne it won't work when the requirements change, and I won't be able to make it work. Therefore the program, will become useless.
* If you give me a program that does not work but is easy to change, then I can make it work, and keep it working as requirements change. Therefore the program will remain continually useful.

There are systems that are practically impossible to change, because the cost of change exceeds the benefit of change.

Eisenhower's Matrix
> I have two kinds of problems, the urgent and the important. The urgent are not important, and the important are never urgent.

Behaviour is urgent but not always particularly important. Architecture is important, but never particularly urgent.

It is the responsibility of software developers to assert the importance of architecture over the urgency of features. It's always a struggle, but remember, as a software developer, you are a stakeholder.

## Part II: Starting with the Bricks: Programming Paradigms
## Chapter 3: Paradigm Overview

Paradigms are ways of programming, relatively unrelated to languages. A paradigm tells you which programming structures to use and when to use them.

There are 3 programming paradigms (and there are unlikely to be any others):
* **structured**: imposes discipline on direct transfer or control (removes go-to statements in favour of ifs/do/while/)
* **object-oriented**: imposes discipline on indirect transfer of control (removes function pointers in favour of classes)
* **functional**: imposes discipline upon assignment (removes assignment, e.g. LISP)

## Chapter 4: Structured Programming

Dijkstra (a dutch smart badass) said: 

> Testing shows the presence, not the absence, of bugs.

In other words, a program can be proven incorrect by a test, but it cannot be proven correct. All tests can do, after sufficient testing effort, is allow us to deem a program to be correct enough for our purposes. A program that is not provable -e.g. do to unrestrained use of `goto`- cannot be deemed correct no matter how many tests are applied to it.

Structured programming forces us to recursively decompose a program into a set of small proable functions. We can then use tests to try to prove those small provable functions incorrect. 

Software architects strive to define modules, components and services that are easily falsifiable (testable).

## Chapter 5: Object-Oriented Programming

A common way to explain the nature of Object-Oriented design is with the terms: encapsulation, inheritance and polymorphism. Perfect encapsulation exists in non-OO languages like C (see below). Inheritance can also be achieved, albeit in a hacky way, in a non-OO language like C. It is the advantages of polymorphism where OO languages shine. Although polymorphism can also be achieved in a non-OO language like C using function pointers, OO languages made polymorphism much more safe and conveninet.

<details>
 <summary> Encapsulation </summary>

 **Premise:** OO languages provide easy and effective encapsulation of data and function. As a result, a line can be drawn around a cohesive set of data and functions. Outside that line, the data is hidden and only some functions are known. We see this concept in action as the private data members and the public member functions of a class.

**Refutation:** Perfect encapsulation exists in non-OO languages like C, as shown in the example below. Users of point.h have no knowledge of the implementation of either the Point data structure or the functions. Then came OO in the form of C++ and perfect encapsulation was broken: because the c++ compiler needs to know the size of the instances of each class, point.h clients know about the implementation details of the Point struct (member variables X and Y). Encapsulation was partially repaired by introducing the `public`, `private` and `protected`.

```C
// point.h
struct Point;
structure Point* makePoint(double x, double y);
double distance(struct Point *p1, struct Point *p2);
```

```C
// point.c
#include "point.h"

struct Point {
 double x, y;
};

structure Ponint* makePoint(double x, double y) {
 sruct Point* p = malloc(sizeof(struct Point));
 p->x = x;
 p->y = y;
 return p;
}

double distance(struct Point* p1, struct Point* p2) {
 double dx = p1->x - p2->x;
 double dy = p1->y - p2->y;
 return sqrt(dx*dx+dy*dy);
}
```
</details>

### Dependency Inversion
Polymorphism in OO languages enabled the use of **plugin architectures**, where low level (utility) functions act as interchangeable components (or plugins) for high level functions. An interface or abstraction layer mediates between them, allowing high-level functions to depend on abstractions rather than concrete implementations.

Without this inversion, control typically flows downward: high-level functions call mid-level functions, which in turn call low-level ones. In such a structure, source code dependencies mirror the flow of control, creating tight coupling and making the system harder to change. By inverting this relationship, high-level functions remain stable and unaffected by changes in lower-level details.

## Chapter 6: Functional Programming

A key charcteristic of functional programming languages is that variables are initialized, but never modified. That is, variables are _inmutable_. When variables are inmutable, problems like race conditions, deadlock conditions and concurrent update problems cannot happen. However, in order to practically implement inmutability, two main compromises need to be made.

### Segregation of Mutability
Divide the application into mutable and inmutable components. The inmutable components perform their tasks in a purely functional way, without using any mutable variables. The inmutable components comunicate with one or more other components that are not purely functional, and allow for the state of variables to be mutated. Mutable variables are protected from concurrent updates or race conditions via a _transactional memory_ scheme (transaction or retry). Architects are advised to push as much processing as possible into the inmutable components, and reduce the amount of code in the components that allow mutations.
  
### Event sourcing
Event sourcing is a strategy wherein we store transactions, but not the state. When state is required, we simply apply all the transactions from the beginning of time. This approach eliminates the possibility of concurrent update issues, since data is only created and read, never updated or deleted. A significant disadvantage of this approach is the huge data-storage capacity required. Shortcuts can be taken, for example saving the state every 24 hrs. Version control systems implement this approach.

## Part III: Design Principles

The SOLID principles tell us how to arrange our functions and data structures into components, and how these components should be interconnected. The goal of the principles is the creation of mid-level software structures that:
* tolerate change
* are easy to understand
* are the basis of components that can be used in many software systems

The SOLID principles are:
* **SRP: Single Responsability Principle** - each software module has one, and only one, reason to change (a single stakeholder or actor).
* **OCP: Open-Closed Principle** - For software to be easy to change, it must be designed to allow changes by adding new code, rather than changing existing code. I.e. software modules should be open for extension, but closed for modification. This allows for stable, tested code to remain untouched and new requirements can be introduces without rewriting.
* **LSP: Liskov Substitution Principle** - To build software systems from interchangeable parts, those parts must adhere to a contract that allows those parts to be substituted for one another. "Is-a" should behave as "is a".
* **ISP: Interface Segregation Principle** - Don’t create interfaces that force classes to implement methods they don’t need. Instead, break them up into smaller, more specific interfaces, tailored to the exact needs of the client.
* **DIP: Dependency Inversion Principle** - High-level modules should not depend on low-level modules. Both should depend on abstractions.
Abstractions should not depend on details. Details should depend on abstractions.



